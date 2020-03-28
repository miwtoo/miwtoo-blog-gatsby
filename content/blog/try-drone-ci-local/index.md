---
title: "ลองทำ CI/CD ด้วย Drone CI บนเครื่องตัวเอง"
author: "Wongsacorn Chugasem"
date: 2019-06-24T07:11:44.334Z
lastmod: 2020-03-27T22:00:28+07:00

description: ""

subtitle: "ปัจจุบันการทำ CI/CD เข้ามาช่วยให้การทำ ซอฟแวร์ง่ายขึ้นเยอะ โดยแต่ล่ะขั้นตอนจะเป็นการทำงานแบบ automated tools ทั้ง build, test ไปจนถึง…"
tags:
 - Drone Ci
 - Ci Cd Pipeline
 - Severless
 - Cncf
 - Github

image: "./images/1.png" 
images:
 - "./images/1.png"
 - "./images/2.png"
 - "./images/3.png"
 - "./images/4.png"
 - "./images/5.png"
 - "./images/6.png"
 - "./images/7.png"
 - "./images/8.png"
 - "./images/9.png"


aliases:
    - "/%E0%B8%A5%E0%B8%AD%E0%B8%87%E0%B8%97%E0%B8%B3-ci-cd-%E0%B8%94%E0%B9%89%E0%B8%A7%E0%B8%A2-drone-ci-%E0%B8%9A%E0%B8%99%E0%B9%80%E0%B8%84%E0%B8%A3%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B8%87%E0%B8%95%E0%B8%B1%E0%B8%A7%E0%B9%80%E0%B8%AD%E0%B8%87-a87e18024ad2"

---

![image](./images/1.png#layoutOutsetCenter)

[https://drone.io/](https://drone.io/)

ปัจจุบันการทำ CI/CD เข้ามาช่วยให้การทำ ซอฟแวร์ง่ายขึ้นเยอะ โดยแต่ล่ะขั้นตอนจะเป็นการทำงานแบบ automated tools ทั้ง build, test ไปจนถึง deploy

โดยส่วนตัวนั้นมีโอกาศได้ลองศึกษา [Drone CI](https://drone.io/) จากโปรเจคที่อาจารย์ให้ไปศึกษา Software ในกลุ่มของ [CNCF](https://landscape.cncf.io/) โดยเลือกมาหนึ่งตัว แล้วก็เลือก [Drone CI](https://drone.io/) มาศึกษา (จากคที่โลโก้มันเหมือนโปเกม่อน 5555) จากนั้นก็มาทำบล็อกใว้เพื่อเก็บใว้เผื่อเก็บใว้อ่านเอง และแชร์สิ่งได้รู้มา

Drone CI สร้างขึ้นในปี 2012 ถือเป็นหนึ่งใน Container-Native ถูกจัดอยู่ในกลุ่มของ Continuous Integration &amp; Delivery และเป็น Open Source มีความสามารถในการ เป็น โปรแกรมทดสอบซอฟแวร์อัตโนมัติ CI/CD ตามที่ถูกจัดกลุ่มใว้

ข้อดีของ Drone CI คือ เขียน ตั้งค่าตัว Pipelines ง่าย และอ่านง่าย โดยที่แต่ล่ะขั้นตอนของ Pipeline จะมี Docker container แยก และดาวโหลอัตโนมัติเวลาที่ทำงาน และ สามารถใช้งานกับ โปรแกรมจัดการโค้ดตัวไหนก็ได้ ทำงานบน แพทฟอร์มไหนก็ได้ ใช้งานกับภาษาไหนก็ได้ที่มี Docker container สนับสนุน รวมทั้งมี Plugins ให้เลือกใช้มากมาย หากไม่พอกับการใช้งานสามารถสร้างเองได้ การ Scale ที่ง่าย และจะ scales อันโนมัติ อีกทั้ง มี cloud ของตัวเอง สามารถใช้งานฟรีได้ทันที

**Feature ที่น่าสนใจ**

สามารถใช้งานกับ Source Code Manager ตัวไหนก็ได้ ที่อยู่บนพื้นฐานของ git เช่น GitHub, GitLab ทั้ง self-host git ก็ยังใช้งานได้ ตัว server รันได้กับทั้ง Linux และ Windows มี ฐานข้อมูลในตัว Sqlite, MySQL, Postgres สามารถทำ Clustering บน Agents, Kubernetes, Nomad ได้ หรืออาจจะรันแบบ Single Machine ก็ได้ สามารถทำ Scheduling บน cron-job ได้ มี Plugins มากมายทั้ง Pipeline Plugins , Custom Secret Plugins, Custom Registry Plugins, Custom Yaml Plugins, Custom Access Control Plugins เก็บความลับของเราด้วยการเข้ารหัสได้หลากหลายแบบ และการ autoscaling บน หลากหลาย แพทฟอร์ม เช่น Amazon EC2, Digital Ocean, Google Compute

วันนี้จะมาลองสร้าง self-hosted ของ Drone บน Windows โดยใช้ [Docker](https://hub.docker.com/editions/community/docker-ce-desktop-windows) และ [ngrok](https://ngrok.com/) โดยเชื่อมต่อกับ Github เพื่อลองศึกษาการทำ self-hosted บนเครื่องของตัวเอง

### **วิธีการติดตั้ง**

#### เครื่องมือที่ใช้

*   _Drone CI_ `_1.0_`
*   _Docker Desktop Community_`_2.0.0.3 (31259)_`
*   _Ngrok_ `2.3.30`

ในทีนี้เป็นการติดตั้ง แบบ Single Machine เชื่อมต่อกับ GitHub สามารถดูเพิ่มเติมได้ [ที่นี้](https://docs.drone.io/installation/github/single-machine/)

ก่อนอื่นเลยให้เรา สร้าง url แบบสาธารณะ โดยใช้ ngrok ด้วยคำสั่ง ผ่าน cmd
> ngrok http 80

จะได้หน้าตาแบบนี้




![image](./images/2.png#layoutTextWidth)

ก่อนที่จะลงรันให้เชื่อมต่อกับ GitHub ได้เราจะติองมี OAuth Application ก่อน เพื่อให้ได้ Client ID และ Client Secret

1.  **สร้าง OAuth Application ใน GitHub**
 ต้องกรอก Authorization callback ให้ตรงตามรูปแบบ ในรูป



![image](./images/3.png#layoutTextWidth)



Application name: ตั้งชื่อได้ตามใจชอบ  
Homepage URL: ใส่ URL ของ ngrok  
Authorization callback URL: ใส่ URL ของ ngrok ตามด้วย /login  
แล้วกด Register application ได้เลย

แล้วจะได้ Client ID และ Client Secret มา เพื่อนำมาใช้ในขั้นตอนต่อไป




![image](./images/4.png#layoutTextWidth)



2. **เขียนไฟล์ docker-compose.yaml**

```yaml
version: '2'

services:
  drone-server:
    container_name: drone
    image: drone/drone:1
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./data:/var/lib/drone/
    restart: always
    environment:
      - DRONE_GIT_ALWAYS_AUTH=false
      - DRONE_TLS_AUTOCERT=false
      - DRONE_GITHUB_SERVER=https://github.com
      - DRONE_GITHUB_CLIENT_ID= กรอก Client ID
      - DRONE_GITHUB_CLIENT_SECRET= กรอก Client Secret
      - DRONE_RUNNER_CAPACITY=2 
      - DRONE_SERVER_HOST=fbeb1796.ngrok.io
      - DRONE_SERVER_PROTO=http
```


โดยให้กรอก Client ID และ Client Secret และ Sever Host จาก URL ของ ngrok โดยไม่ต้องใส่ protocol ใว้ข้างหน้า

จากนั้นสั้ง
> docker-compose up

จะได้หน้าตาประมาณนี้



![image](./images/5.png#layoutOutsetCenter)

### **วิธีการใช้งานเบื้องต้น**

1.  **เข้าเว็บไซต์ของเราที่เชื่อมต่อกับ server 
**จาก URL ของ ngrok



![image](./images/6.png#layoutTextWidth)



2. กดเข้าไปที่ repository ที่ต้องการ แล้วกด Activate repository




![image](./images/7.png#layoutTextWidth)



3. สร้างไฟล์ .drone.yml ใน repository (ตัวอย่าง การ install, test และ build Angular จาก นั้น deploy ไปที่ github page)


```yaml
kind: pipeline
name: default

steps:
- name: install
  image: johnpapa/angular-cli
  commands:
    - npm install

- name: test
  image: trion/ng-cli-karma:6.2.1
  commands:
    - ng test --progress false --watch false

- name: build
  image: johnpapa/angular-cli
  commands:
    - ng build --prod --base-href "https://miwtoo.github.io/drone-angular-git-test/"

- name: publish  
  image: plugins/gh-pages
  settings:
    username:
      from_secret: github_username
    password:
      from_secret: github_password    
    pages_directory: dist/drone-angular-git-test
```


ในที่นี้ในส่วนของ usrname และ password ของ github มีการใส่ secret ใว้เพื่อเป็นการที่เราไม่ต้องกรอก เข้าไปตรงๆ จะทำให้คนอื่นรู้รหัสเราได้ สามารถเข้าไปเพิ่มได้ที่ setting




![image](./images/8.png#layoutTextWidth)



4. จากนั้น push ขึ้น drone จะทำตามขั้นตอนที่เราเขียน script ใว้




![image](./images/9.png#layoutTextWidth)



อ้างอิง   
 [https://docs.drone.io/installation/github/single-machine/](https://docs.drone.io/installation/github/single-machine/)

[https://medium.com/@jccguimaraes/run-a-drone-ci-pipeline-locally-f4bfb4741c53](https://medium.com/@jccguimaraes/run-a-drone-ci-pipeline-locally-f4bfb4741c53)

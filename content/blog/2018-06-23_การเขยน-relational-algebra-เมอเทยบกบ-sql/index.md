---
title: "การเขียน Relational Algebra เมื่อเทียบกับ SQL"
author: "Wongsacorn Chugasem"
date: 2018-06-23T17:22:48.324Z
lastmod: 2020-03-27T22:00:22+07:00

description: ""

subtitle: "Relation Algebra คืออะไร ?"

image: "./images/4.png" 
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
 - "./images/10.png"


aliases:
    - "/%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B9%80%E0%B8%82%E0%B8%B5%E0%B8%A2%E0%B8%99-relational-algebra-%E0%B9%80%E0%B8%A1%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B9%80%E0%B8%97%E0%B8%B5%E0%B8%A2%E0%B8%9A%E0%B8%81%E0%B8%B1%E0%B8%9A-sql-623f4e7ba448"

---

### Relation Algebra คืออะไร ?

เป็นการเขียนความสัมพันธ์ของตาราง ในรูปของสัญลักษณ์ ซึ่งถือกำเนิดก่อนที่จะมีภาษา SQL ให้ใช้กันเสียอีก
> ข้อตกลง — เนื่องจากใน Medium เขียน ให้อยู่ด้านล่างไม่ได้ จึงต้องเขียนแบบติดกันปกติ> เช่น ต้องการจะเขียนแบบนี้



![image](./images/1.png#layoutTextWidth)

> แต่ต้องเขียน แบบนี้> **π a,b (R)**

### สัญลักษณ์พื้นฐาน

σ — Selection เป็นการเลือกแถวในตารางแบบมีเงื่อไขได้
> σ **_เงื่อนไข_** (**_ชื่อตาราง_**)

ใน SQL
> SELECT * FROM **_ชื่อตาราง_** WHERE **_เงื่อนไข;_**



![image](./images/2.png#layoutTextWidth)



π — Projection เป็นการเลือกคอลัมน์ ปกติ
> π **_ชื่อคอลัมน์_** (**_ชื่อตาราง_**)

ใน SQL
> SELECT DISTINCT **_ชื่อคอลัมน์_** FROM **_ชื่อตาราง;_**



![image](./images/3.png#layoutTextWidth)



⨯ — Cross-product เป็นการรวม 2 ตาราง
> **_(ตารางที่ 1)_** ⨯ (**ตารางที่ 2)**

ใน SQL
> SELECT * FROM **_ตารางที่ 1, ตารางที่ 2;_**



![image](./images/4.png#layoutTextWidth)



-(เครื่องหมายลบ) — Set-difference อยู่ใน ตารางที่ 1 แต่ไม่อยู่ในตารางที่ 2
> **_(ตารางที่ 1)_** -(**ตารางที่ 2)**

ใน SQL
> SELECT ชื่อคอลัมน์ FROM **_ตารางที่ 1_**  
> MINUS  
> SELECT ชื่อคอลัมน์ FROM **ตารางที่ 2**;



![image](./images/5.png#layoutTextWidth)



∪ — Union การรวม 2 ตารางเข้าด้วยกัน
> **_(ตารางที่ 1)_** ∪ (**ตารางที่ 2)**

ใน SQL
> SELECT ชื่อคอลัมน์ FROM **_ตารางที่ 1_**  
> UNION  
> SELECTชื่อคอลัมน์ FROM **ตารางที่ 2**;



![image](./images/6.png#layoutTextWidth)



∩— Intersection นำค่าที่ 2 ตารางมีเหมือนกัน
> **_(ตารางที่ 1)_** ∩ (**ตารางที่ 2)**

ใน SQL
> SELECT ชื่อคอลัมน์ FROM **_ตารางที่ 1_**  
> INTERSECT  
> SELECTชื่อคอลัมน์ FROM **ตารางที่ 2**;



![image](./images/7.png#layoutTextWidth)



⨝ —Join เป็นการรวม 2 ตาราง แต่จะมีการเชื่อม Key ให้ด้วย โดยให้ผลลัพต่างจาก Cross-product เพราะ Cross-product จะนำทุกค่ามา กระจายให้ครบกัน แต่ Join จะแสดงแค่ค่าที่มีการเชื่อมกัน
> **_(ตารางที่ 1)_** ⨝ (**ตารางที่ 2)**

ใน SQL
> SELECT * FROM **_ตารางที่ 1_**,**ตารางที่ 2**  
> WHERE **ตารางที่ 1**.id = **ตารางที่ 2**.id;



![image](./images/8.png#layoutTextWidth)



/ — Division จะนำเฉพาะค่าที่ ค่าที่ฝั่งซ้ายของเครื่องหมายมี




![image](./images/9.png#layoutTextWidth)



เช่นในภาพ จะสามารถ แสดงการเชื่อมได้ดังนี้




![image](./images/10.png#layoutTextWidth)



นั้นคือ ผลลัพของ จากการ V / W จะได้ a และ b ที่มี 1 **และ** 2 ในกิ่ง
> **_(คอลัมน์ 1)_** /(**คอลัมน์ 2)**

ใน SQL
> SELECT คอลัมน์ 1 FROM ชื่อตาราง WHERE คอลัมน์ 2 = ค่าที่ 1 ANDคอลัมน์ 2 = ค่าที่ 2 AND ….

ปล. สามารถทดลองเล่น Relational Algebra ได้ ที่ [https://dbis-uibk.github.io/relax/calc.htm](https://dbis-uibk.github.io/relax/calc.htm)

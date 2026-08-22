# สคริปวิดีโอ: Data Warehouse Construction Tutorial (Northwind)

**ความยาวรวม:** ~18-20 นาที
**โปรเจกต์อ้างอิง:** `DW_lab_6730202477-dw_duckdb` / dbt project: `northwind`
**Repo:** github.com/chawanakorn-phet/lab_video (branch: `dw`)

> หมายเหตุ: สคริปนี้แบ่งเป็น 4 ส่วนตามเงื่อนไขโจทย์ ถ้ากลุ่มมีสมาชิกกี่คนก็แบ่งคนละส่วนได้เลย (เช่น 4 คน = คนละ 1 ส่วน) ทุกคนควรพูดแนะนำตัวสั้นๆ ช่วงเปิดวิดีโอ เพื่อโชว์ว่า "สมาชิกทุกคนมีส่วนร่วม"

---

## เปิดวิดีโอ (0:00 - 1:00)

**[โชว์หน้าจอ: สไลด์ชื่อโปรเจกต์ หรือหน้า VS Code ของโปรเจกต์]**

> "สวัสดีครับ/ค่ะ พวกเรากลุ่ม [ใส่ชื่อกลุ่ม] มาจากวิชา Data Warehouse
> วันนี้จะมาสาธิตวิธีการสร้าง Data Warehouse จากฐานข้อมูล Northwind
> โดยใช้ Python, dbt และ DuckDB ครับ/ค่ะ"

**[สมาชิกแต่ละคนแนะนำตัวสั้นๆ ทีละคน คนละ 1 ประโยค]**

> "เนื้อหาวันนี้แบ่งเป็น 4 ส่วนคือ
> 1. การสร้าง Python environment
> 2. การใช้งาน Git และ GitHub
> 3. การใช้งาน dbt เบื้องต้น
> 4. การสร้าง Data Warehouse จริง พร้อมแดชบอร์ดแสดงผล"

---

## ส่วนที่ 1: การสร้าง Python Environment (1:00 - 5:00)

**[โชว์หน้าจอ: เปิด terminal ที่โฟลเดอร์โปรเจกต์]**

> "เริ่มจากการสร้าง virtual environment กันก่อน เพื่อแยก dependency ของโปรเจกต์นี้
> ออกจาก Python ตัวหลักในเครื่อง"

**[พิมพ์คำสั่งทีละบรรทัด พร้อมอธิบาย]**

```bash
python -m venv .venv
```

> "คำสั่งนี้จะสร้างโฟลเดอร์ .venv ขึ้นมา เก็บ Python environment แยกต่างหาก"

```bash
.venv\Scripts\activate
```

> "จากนั้น activate environment — จะสังเกตได้ว่าหน้า prompt จะมีวงเล็บ (.venv)
> ขึ้นมา แปลว่าเราอยู่ใน environment นี้แล้ว"

**[โชว์: สร้างไฟล์ .gitignore]**

> "ก่อนติดตั้งอะไร เราสร้างไฟล์ .gitignore ไว้ก่อน เพื่อไม่ให้ Git track
> ไฟล์ที่ไม่จำเป็น เช่นตัว .venv เอง, ไฟล์ฐานข้อมูล .duckdb, และ log ต่างๆ"

```
.venv/
__pycache__/
*.duckdb
target/
logs/
```

**[กลับมาที่ terminal]**

> "ต่อไป upgrade pip ให้เป็นเวอร์ชันล่าสุดก่อน"

```bash
python -m pip install --upgrade pip
```

> "จากนั้นติดตั้งไลบรารีหลักที่ต้องใช้ คือ pandas สำหรับจัดการข้อมูล
> และ dbt-duckdb สำหรับสร้าง data warehouse บน DuckDB"

```bash
pip install pandas dbt-duckdb
```

**[รอให้ install เสร็จ — ตัด/fast-forward ได้]**

> "หลังติดตั้งเสร็จ เรา freeze รายการ package ทั้งหมดเก็บไว้ใน requirements.txt
> เพื่อให้คนอื่นที่ clone โปรเจกต์นี้ไปสามารถติดตั้ง environment เดียวกันได้"

```bash
pip freeze > requirements.txt
```

**[โชว์เนื้อหาไฟล์ requirements.txt สั้นๆ]**

> "เท่านี้ Python environment ของเราก็พร้อมใช้งานแล้วครับ/ค่ะ"

---

## ส่วนที่ 2: การใช้งาน Git และ GitHub (5:00 - 9:00)

**[โชว์หน้าจอ: terminal ในโฟลเดอร์โปรเจกต์]**

> "ต่อไปเป็นเรื่อง Git และ GitHub ซึ่งเราใช้สำหรับ version control
> และเก็บโค้ดไว้บน cloud ให้ทุกคนในกลุ่มทำงานร่วมกันได้"

```bash
git init
```

> "คำสั่งนี้จะเริ่มต้น Git repository ในโฟลเดอร์นี้"

**[ตั้งค่าตัวตนของผู้ commit ถ้ายังไม่เคยตั้ง — พูดสั้นๆ ว่าทำครั้งเดียวต่อเครื่อง]**

```bash
git config --global user.name "ชื่อ-GitHub"
git config --global user.email "อีเมลที่ผูกกับ GitHub"
```

> "ก่อนเริ่มทำงานจริง เราสร้าง branch ใหม่ขึ้นมาแยกจาก main
> เพื่อไม่ให้กระทบโค้ดหลักระหว่างที่เรายังพัฒนาไม่เสร็จ"

```bash
git checkout -b dw
```

> "ตอนนี้เราอยู่บน branch ชื่อ dw แล้ว ทุกอย่างที่เราทำต่อไปนี้
> จะถูกเก็บอยู่บน branch นี้ก่อน"

**[ทำงานตามขั้นตอนต่างๆ ในส่วนที่ 1, 3, 4 ให้เสร็จก่อน แล้วค่อยกลับมาส่วนนี้ต่อ]**

> "เมื่อทำงานเสร็จแล้ว เราจะ add ไฟล์ทั้งหมดเข้า staging area"

```bash
git add .
```

> "แล้ว commit พร้อมข้อความอธิบายว่าเราทำอะไรไปบ้าง"

```bash
git commit -m "Build Northwind data warehouse pipeline with dbt-duckdb and Streamlit apps"
```

> "จากนั้น push ขึ้นไปที่ GitHub ที่เราสร้าง remote repository ไว้แล้ว"

```bash
git remote add origin https://github.com/<username>/<repo>.git
git push -u origin dw
```

**[โชว์หน้าเว็บ GitHub repo ว่ามี branch dw ขึ้นไปแล้ว]**

> "สุดท้ายเมื่อมั่นใจว่าโค้ดบน branch dw ใช้งานได้ถูกต้อง เราก็ merge
> เข้า main branch เพื่อให้เป็นเวอร์ชันหลักของโปรเจกต์"

```bash
git checkout main
git merge dw
git push origin main
```

**[โชว์หน้า GitHub ว่า main อัปเดตแล้ว]**

> "เท่านี้ขั้นตอน Git และ GitHub ของเราก็เรียบร้อยครับ/ค่ะ"

---

## ส่วนที่ 3: การใช้งาน dbt เบื้องต้น (9:00 - 14:00)

**[โชว์หน้าจอ: โครงสร้างโฟลเดอร์ northwind]**

> "ต่อไปเป็นหัวใจหลักของวันนี้ คือการใช้งาน dbt เบื้องต้น
> dbt ย่อมาจาก data build tool ใช้สำหรับแปลงข้อมูลดิบให้กลายเป็น
> ตารางที่พร้อมใช้งานวิเคราะห์ ด้วยการเขียน SQL เป็นหลัก"

**[โชว์ dbt_project.yml]**

> "โปรเจกต์ dbt ของเราชื่อ northwind ตั้งค่าให้โมเดลในโฟลเดอร์
> staging และ datawarehouse ถูก materialize เป็น table ทั้งหมด"

**[โชว์โฟลเดอร์ datasets/]**

> "เราเอาไฟล์ CSV ของฐานข้อมูล Northwind ทั้ง 20 ตาราง มาวางไว้ในโฟลเดอร์
> datasets/ เช่น customer, orders, order_details, products, employees เป็นต้น"

**[โชว์ src_northwind.yml]**

> "แล้วประกาศ source เหล่านี้ไว้ในไฟล์ src_northwind.yml
> เพื่อให้ dbt รู้จักว่าตารางต้นทางของเราคือไฟล์อะไรบ้าง"

**[โชว์ตัวอย่าง stg_customers.sql]**

> "ในโฟลเดอร์ staging เราสร้างไฟล์ SQL หนึ่งไฟล์ต่อหนึ่งตารางต้นทาง
> เช่น stg_customers.sql ก็จะดึงข้อมูลจาก source customer มา แล้วเพิ่ม
> คอลัมน์ ingestion_timestamp เพื่อบันทึกเวลาที่โหลดข้อมูลเข้ามา"

**[โชว์ stg_products.sql และพูดถึงการจัดการ products]**

> "สำหรับตาราง products เราต้องจัดการเป็นพิเศษ เพราะบางแถวมีคอลัมน์
> supplier_ids ที่เก็บซัพพลายเออร์หลายรายคั่นด้วยเครื่องหมาย semicolon
> เราจะจัดการเรื่องนี้ตอนสร้าง dimension ต่อไป"

**[ก่อนรัน ต้องตั้งค่า profile — โชว์ไฟล์ profiles.yml]**

> "ก่อนรัน dbt ได้ ต้องมีไฟล์ profiles.yml ที่บอกว่าจะเชื่อมต่อฐานข้อมูล
> แบบไหน ในที่นี้เราใช้ DuckDB โดยกำหนด path ไปที่ไฟล์ dev.duckdb"

```yaml
northwind:
  target: dev
  outputs:
    dev:
      type: duckdb
      path: dev.duckdb
      threads: 4
```

**[กลับมาที่ terminal เข้าไปในโฟลเดอร์ northwind]**

```bash
cd northwind
dbt debug
```

> "คำสั่ง dbt debug เอาไว้เช็คว่าการตั้งค่าทุกอย่างถูกต้อง เชื่อมต่อฐานข้อมูล
> ได้จริง ถ้าเห็นคำว่า All checks passed! แปลว่าพร้อมรันแล้ว"

```bash
dbt run
```

> "คำสั่งนี้จะรันทุกโมเดลตามลำดับ dependency ที่ถูกต้องให้อัตโนมัติ
> จะเห็นว่า dbt สร้างตาราง staging ทั้งหมดก่อน แล้วค่อยไปสร้างตาราง
> dimension และ fact ต่อ"

**[โชว์ผลลัพธ์ terminal: PASS=25]**

> "และเราสามารถรัน dbt test เพื่อเช็คคุณภาพข้อมูล เช่น ตรวจว่า
> customer_id ต้องไม่ซ้ำและต้องไม่เป็นค่าว่าง"

```bash
dbt test
```

---

## ส่วนที่ 4: การสร้าง Data Warehouse (14:00 - 19:00)

**[โชว์โฟลเดอร์ models/datawarehouse/]**

> "ในโฟลเดอร์ datawarehouse เราออกแบบโมเดลตามหลัก dimensional modeling
> คือแยกเป็นตาราง dimension สำหรับอธิบายข้อมูล และตาราง fact
> สำหรับเก็บตัวเลขธุรกรรม"

**[โชว์ dim_customers.sql, dim_products.sql, dim_employees.sql, dim_date.sql]**

> "เรามี 4 dimension หลัก คือ
> dim_customers เก็บข้อมูลลูกค้า
> dim_products เก็บข้อมูลสินค้า — ตรงนี้เราแก้ปัญหา supplier_ids
> ที่มีหลายค่า โดยใช้ split_part ดึงซัพพลายเออร์รายแรกมาแมพกับตาราง suppliers
> dim_employees เก็บข้อมูลพนักงาน
> และ dim_date เป็นตาราง calendar ที่สร้างด้วย generate_series
> ครอบคลุมตั้งแต่ปี 2014 ถึง 2050 พร้อมข้อมูลปี ไตรมาส เดือน และวันในสัปดาห์"

**[โชว์ fact_sales.sql]**

> "ส่วน fact_sales คือตารางหัวใจของ data warehouse เรา รวมข้อมูลจาก
> orders กับ order_details เข้าด้วยกัน เก็บ quantity, unit_price, discount
> พร้อมทำ deduplicate ด้วย ROW_NUMBER เพื่อให้แน่ใจว่าไม่มีแถวซ้ำ"

**[โชว์ schema.yaml]**

> "และเราเขียน test ไว้ใน schema.yaml เพื่อยืนยันความถูกต้องของ
> primary key ในตาราง dimension ด้วย"

**[กลับ terminal รัน dbt run ให้เห็นว่าสร้างสำเร็จ]**

```bash
dbt run
```

**[เปิด query_duckdb.py หรือรันเพื่อ preview ข้อมูล]**

> "เราเขียนสคริปต์ query_duckdb.py ไว้สำหรับดึงข้อมูลออกมาดูตรวจสอบเร็วๆ
> ผ่าน Python และ pandas"

```bash
python query_duckdb.py
```

**[โชว์ app.py — เปิด browser ที่ localhost:8501]**

> "นอกจากนี้เรายังทำหน้าเว็บด้วย Streamlit ชื่อ app.py สำหรับสำรวจ
> ตารางทั้งหมดใน data warehouse ดูจำนวนแถว ดู schema แต่ละคอลัมน์
> และ preview ข้อมูลได้ทันที"

```bash
pip install streamlit
streamlit run app.py
```

**[โชว์หน้าเว็บ: แท็บ Database Schema & Overview และ Data Viewer]**

> "จะเห็นว่าตอนนี้เรามีทั้งหมด 25 ตาราง รวมกว่า 13,800 แถว
> ครอบคลุมทุกขั้นตอนตั้งแต่ staging จนถึง data warehouse"

**[เปิด dashboard_app.py — เปิด browser อีกพอร์ตหนึ่ง]**

> "สุดท้าย เราสร้างแดชบอร์ดวิเคราะห์ยอดขายแบบ OLAP ด้วย dashboard_app.py
> ที่ join fact_sales เข้ากับ dimension ทั้งหมด และให้ผู้ใช้เลือกมุมมองข้อมูล
> ได้หลายระดับ เช่น ปี ไตรมาส เดือน วัน หรือแยกตามกลุ่มสินค้า"

```bash
streamlit run dashboard_app.py
```

**[โชว์หน้าแดชบอร์ด: ตัวกรองด้านซ้าย, กราฟยอดขายตามเวลา, กราฟยอดขายตามสินค้า]**

> "ผู้ใช้สามารถกรองช่วงวันที่ เลือกลูกค้า พนักงาน หรือสินค้าที่สนใจได้
> แดชบอร์ดจะอัปเดตยอดขายรวม จำนวนสินค้า จำนวนออเดอร์ และมูลค่าเฉลี่ย
> ต่อออเดอร์ให้ทันที"

---

## ปิดวิดีโอ (19:00 - 20:00)

**[กลับมาที่สไลด์สรุป หรือหน้ากล้องสมาชิกทุกคน]**

> "สรุปวันนี้พวกเราได้สาธิตขั้นตอนการสร้าง Data Warehouse จากฐานข้อมูล
> Northwind ครบทั้ง 4 ส่วน ตั้งแต่การเตรียม Python environment
> การจัดการเวอร์ชันโค้ดด้วย Git และ GitHub การแปลงข้อมูลด้วย dbt
> ไปจนถึงการสร้าง dimension, fact table และแดชบอร์ดแสดงผลจริง"

> "ขอบคุณที่รับชมครับ/ค่ะ"

---

## เช็คลิสต์ก่อนอัดจริง

- [ ] ทดสอบรันทุกคำสั่งไว้ล่วงหน้า ให้แน่ใจว่าไม่ error กลางไลฟ์
- [ ] เคลียร์หน้าจอ / ปิด terminal ที่ไม่เกี่ยวข้อง
- [ ] เตรียมไฟล์/โฟลเดอร์ให้พร้อมก่อนกด record (จะได้ไม่เสียเวลาหาไฟล์)
- [ ] แบ่งเวลาพูดแต่ละคนให้สมดุล ไม่เกิน 20 นาทีรวม
- [ ] อัปโหลดเป็นลิงก์ YouTube (ตั้งค่า Unlisted หรือ Public ตามที่อาจารย์กำหนด)

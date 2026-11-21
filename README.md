# PublicBank EA – Personal Finance + Calendar

แอปบันทึกรายรับรายจ่าย คุมงบ และปฏิทิน/สิ่งที่ต้องทำ ใช้บนมือถือผ่าน GitHub Pages (ฟรี) และเก็บข้อมูลใน Google Sheet ของคุณเอง (ฟรี)

## ไฟล์ในชุดนี้
| ไฟล์ | ใช้ทำอะไร |
|---|---|
| `index.html` | ตัวแอป (Overview, History, Budget, Calendar) อัปโหลดขึ้น GitHub |
| `Code.gs` | ตัวเชื่อม Google Sheet วางใน Apps Script |
| `README.md` | คู่มือนี้ |

## ส่วนที่ 1 – Google Sheet (เก็บข้อมูล)
1. เปิด https://sheets.new สร้าง Google Sheet ใหม่ ตั้งชื่อ เช่น `PublicBank EA Data`
2. เมนู **Extensions › Apps Script** ลบโค้ดเดิมทั้งหมด แล้ววางโค้ดจาก `Code.gs`
3. แก้บรรทัด `const SECRET_KEY = '...'` เป็นรหัสลับของคุณเอง (ตัวอักษรและตัวเลข 12 ตัวขึ้นไป) แล้วกด 💾 Save
4. กด **Deploy › New deployment** › ไอคอนเฟือง › เลือก **Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
   - กด **Deploy** › **Authorize access** › เลือกบัญชี Google › ถ้าเจอ "Google hasn't verified this app" ให้กด **Advanced › Go to … (unsafe)** (เป็นสคริปต์ของคุณเอง) › **Allow**
5. คัดลอก **Web app URL** (ลงท้ายด้วย `/exec`) เก็บไว้

> แก้ Code.gs ภายหลัง (หรือเคยวาง Code.gs ชุดเก่าไว้แล้ว): วางโค้ดใหม่ › Save › **Deploy › Manage deployments › ✏️ › Version: New version › Deploy** URL จะเหมือนเดิม

## ส่วนที่ 2 – GitHub Pages (เปิดแอป)
1. สมัคร/ล็อกอิน https://github.com แล้วไปที่ https://github.com/new
2. Repository name: `publicbank-ea` · เลือก **Public** · กด **Create repository**
3. กด **uploading an existing file** › ลาก `index.html` และ `README.md` ใส่ › **Commit changes**
4. ไปที่ **Settings › Pages** › Source: **Deploy from a branch** › Branch: **main** / **(root)** › **Save**
5. รอ 1–2 นาที แอปจะอยู่ที่ `https://<ชื่อผู้ใช้>.github.io/publicbank-ea/`

> อัปเดตแอปภายหลัง: เข้า repo › กด `index.html` › ไอคอน ⋯ › **Upload files** ลากไฟล์ใหม่ทับ › Commit

## ส่วนที่ 3 – เชื่อมแอปกับ Google Sheet
1. เปิดลิงก์แอปใน Safari/Chrome บนมือถือ › หน้า Overview กด **Statement** › การ์ด **Google Sheets Sync**
2. วาง Web app URL และรหัสลับ (SECRET_KEY) › กด **Connect & Sync**
3. ขึ้น "Synced with Google Sheets ✓" แปลว่าสำเร็จ ทุกเครื่องที่ใช้ต้องทำขั้นนี้ครั้งเดียว
4. iPhone: ปุ่มแชร์ › **เพิ่มไปยังหน้าจอโฮม** · Android: เมนู ⋮ › **เพิ่มลงในหน้าจอหลัก**

## ย้ายข้อมูลจากแอปเดิม (artifact บน claude.ai)
1. เปิดแอปเดิม › Statement › **Download Full Backup** (ไฟล์นี้มีทั้งรายการเงินและรายการในปฏิทิน)
2. เปิดแอปใหม่ (เชื่อม Sheet แล้ว) › Statement › **Restore from File** › เลือกไฟล์ › Restore
3. ข้อมูลจะถูกส่งขึ้น Google Sheet ให้อัตโนมัติ

## ใน Google Sheet จะเห็น
- แท็บ **Transactions**: 1 แถวต่อ 1 รายการเงิน (วันที่ ประเภท หมวด จำนวนเงิน หมายเหตุ) ใช้ทำ Pivot/กราฟได้
- แท็บ **Planner**: 1 แถวต่อ 1 รายการในปฏิทิน (วันที่ เวลา ชื่อ การทำซ้ำ แท็ก สถานะ หมายเหตุ)
- แท็บ **Settings**: งบ ลิมิต เป้าหมาย หมวดที่สร้างเอง (แอปใช้ ไม่ต้องแก้เอง)
- ควรแก้ข้อมูลผ่านแอป ถ้าจะแก้ในชีตโดยตรง:
  - Transactions: แก้ได้เฉพาะ Amount / Note / Date (รูปแบบ YYYY-MM-DD)
  - Planner: แก้ได้เฉพาะ Title / Date (YYYY-MM-DD) / Time (HH:MM) / Note และห้ามแก้คอลัมน์ Data
  - แอปจะเห็นการแก้ไขเมื่อเปิดแอปใหม่หรือสลับกลับมาที่แอป

## ใช้แบบออฟไลน์ได้ไหม
ได้ ถ้าไม่มีเน็ต แอปจะเก็บไว้ในเครื่องก่อน แล้วส่งขึ้น Sheet ให้เองเมื่อออนไลน์ (สถานะจะขึ้นสีเหลือง/แดงในหน้า Statement)

## ความปลอดภัย
- โค้ดแอปบน GitHub เป็นสาธารณะ แต่ **ไม่มีข้อมูลการเงินอยู่ในนั้น** ข้อมูลอยู่ใน Google Sheet ส่วนตัวของคุณ
- ใครจะอ่าน/เขียนข้อมูลได้ต้องมีทั้ง URL และรหัสลับ อย่าใส่รหัสลับลงในไฟล์ที่อัปโหลดขึ้น GitHub และอย่าแชร์ให้คนอื่น
- ถ้าสงสัยว่ารหัสรั่ว: เปลี่ยน SECRET_KEY ใน Code.gs › Deploy เวอร์ชันใหม่ › ใส่รหัสใหม่ในแอปทุกเครื่อง

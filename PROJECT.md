# Hondana (本棚) - Manga & Light Novel Digital Hub

ระบบคลังหนังสือดิจิทัล (E-Book Store) สำหรับมังงะและไลท์โนเวลแปลไทยถูกลิขสิทธิ์ ออกแบบและพัฒนาขึ้นเพื่อจัดการระบบซื้อขาย ตรวจสอบสิทธิ์การดาวน์โหลด และบริหารจัดการข้อมูลหลังบ้านอย่างครบวงจร

---

## 👥 ข้อมูลสมาชิกผู้จัดทำ (Developer Profile)
- **67332110028-1** นายปฏิพัทธ์ บัวแสง
- **67332110063-2** นายกิตติธร รูปสะอาด
- **สาขาวิชา:** วิศวกรรมคอมพิวเตอร์ (Computer Engineering) ชั้นปีที่ 3
- **สถาบัน:** มหาวิทยาลัยเทคโนโลยีราชมงคลอีสาน วิทยาเขตขอนแก่น (RMUTI Khon Kaen)

---

## 🏗 สถาปัตยกรรมและเทคโนโลยีที่ใช้ (Tech Stack)

### 1. Backend & Server
- **Python (Standard Library & HTTP Server):** ใช้โมดูล `http.server` และ `socketserver` ในการสร้าง RESTful API เสมือนแบ็กเอนด์เซิร์ฟเวอร์จริง เพื่อให้เข้าใจกลไกของ HTTP Request/Response, Routing และการจัดการฐานข้อมูลเชิงลึกโดยไม่พึ่งพา Framework สำเร็จรูป
- **Database Architecture (SQLite):** ออกแบบฐานข้อมูล Relational Database ตามหลักการ **3NF (Third Normal Form)** อย่างเคร่งครัด เพื่อลดความซ้ำซ้อนและรักษาความถูกต้องของข้อมูล (Data Integrity)

### 2. Frontend & UI Design
- **HTML5 / CSS3 / JavaScript (Vanilla ES6+):** พัฒนาฝั่ง Client-side แบบ Single-Page Application (SPA) สลับแท็บการทำงานลื่นไหล ไม่มีสะดุด
- **Tailwind CSS (CDN):** ตกแต่งหน้าตาเว็บไซต์ด้วยดีไซน์โหมดมืดสไตล์พรีเมียม (Dark Vintage / Cyberpunk Minimal) รองรับการแสดงผลทุกขนาดหน้าจอ (Responsive Design)

---


## 🗄️ โครงสร้างฐานข้อมูล SQLite 3NF Schema (11 Tables)
1. `roles` (role_id, role_name)
2. `users` (user_id, email, password_hash, full_name, phone, role_id, created_at)
3. `categories` (category_id, category_name)
4. `authors` (author_id, author_name, bio)
5. `ebooks` (ebook_id, title, description, price, cover_image_url, is_active, category_id, author_id)
6. `carts` (cart_id, user_id, updated_at)
7. `cart_items` (cart_item_id, cart_id, ebook_id, quantity)
8. `orders` (order_id, order_code, user_id, order_date, total_amount, status)
9. `order_items` (order_item_id, order_id, ebook_id, quantity, unit_price)
10. `payments` (payment_id, order_id, payment_method, proof_image, status, paid_at)
11. `download_links` (download_id, order_id, ebook_id, file_name, file_size, download_url, expires_at)

---

## 🚀 ฟีเจอร์หลักของระบบ (Key Features)

### 👤 ฝั่งลูกค้า (Customer / Member)
- **ระบบสมาชิก & PDPA Compliance:** สมัครสมาชิกพร้อมกดยอมรับนโยบายคุ้มครองข้อมูลส่วนบุคคล มีการบันทึกประวัติการให้ความยินยอม
- **ระบบค้นหาและกรองข้อมูล:** ค้นหาหนังสือแบบ Real-time และคัดแยกตามหมวดหมู่
- **ระบบตะกร้าสินค้า & ชำระเงิน (Checkout):** ระบบจำลองสแกน QR Code PromptPay, แนบสลิปโอนเงิน และติดตามสถานะออเดอร์
- **ชั้นหนังสือของฉัน (My Library):** คลังหนังสือส่วนตัวเฉพาะเล่มที่เป็นเจ้าของ สามารถตรวจสอบและดาวน์โหลดไฟล์อ่านได้ตลอด 24 ชม.

### 🛠️ ฝั่งผู้ดูแลระบบ (Admin Backoffice)
- **ระบบตรวจสอบคำสั่งซื้อและสลิป:** ตรวจสอบรูปภาพสลิปของลูกค้า กดอนุมัติ (`confirmed`) เพื่อออกสิทธิ์ดาวน์โหลดอัตโนมัติ หรือปฏิเสธ (`cancelled`) กรณีสลิปไม่ถูกต้อง
- **ระบบจัดการคลังหนังสือ:** เพิ่ม ลบ แก้ไขข้อมูลมังงะ, ปรับสถานะเปิด/ปิดการขาย และมีฟีเจอร์ช่วยหมุนภาพปกหนังสือ (Image Rotation)
- **ระบบจัดการหมวดหมู่และผู้ใช้งาน:** จัดการหมวดหมู่, ปรับเปลี่ยนบทบาทผู้ใช้ (Role Management: Admin / Customer) และระบบแจ้งเตือนพฤติกรรมลูกค้าที่เคยถูกปฏิเสธสลิป
- **ระบบรายงานและวิเคราะห์ข้อมูลเชิงธุรกิจ (Business Analytics):**
  - **Report 1:** ยอดขายตามช่วงเวลา (Sales Over Time) พร้อมยอดเฉลี่ยต่อออเดอร์
  - **Report 2:** E-Book ขายดีที่สุด Top 5 Best-Sellers
  - **Report 3:** ยอดขายตามหมวดหมู่ พร้อมระบบ Drilldown เจาะลึกดูรายการหนังสือย่อย
  - **Report 4:** วิเคราะห์พฤติกรรมลูกค้าและยอดซื้อสะสม
- **ระบบส่งออกรายงาน CSV Export:** แปลงข้อมูลตารางรายงานเชิงวิเคราะห์เป็นไฟล์ CSV พร้อมดาวน์โหลดนำไปใช้งานต่อได้ทันที

---

## ⚙️ วิธีการติดตั้งและรันโปรเจกต์

1. โคลน Repository นี้ลงในเครื่องของคุณ:
   ```bash
   git clone [https://github.com/Patipat-t/ProjectDB.git](https://github.com/Patipat-t/ProjectDB.git)
   cd ProjectDB

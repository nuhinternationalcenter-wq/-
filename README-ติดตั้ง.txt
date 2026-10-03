ระบบกระทบยอด บน Cloudflare (ไม่ต้องล็อกอิน Claude)
======================================================
ฐานข้อมูล D1 ชื่อ "reconcile-db" สร้างไว้ในบัญชี Cloudflare ของคุณแล้ว
ไฟล์ worker.js = หน้าเว็บ + ระบบหลังบ้าน รวมไว้ในไฟล์เดียว (มีข้อมูลตั้งต้นจากลิงก์เดิมติดไปด้วย)

วิธีที่ 1: ผ่านหน้าเว็บ Cloudflare (ไม่ต้องติดตั้งโปรแกรม)
1) เข้า dash.cloudflare.com > Workers & Pages > Create > Start with Hello World
   ตั้งชื่อ worker > Deploy
2) กด Edit code > ลบโค้ดเดิมทั้งหมด > เปิด worker.js ด้วย Notepad กด Ctrl+A, Ctrl+C แล้ววาง > Deploy
3) กลับไปหน้า Worker > Settings > Bindings > Add > D1 database
   Variable name: DB   ·   D1 database: reconcile-db  > Deploy
4) Settings > Variables and Secrets > Add > Type: Secret
   Variable name: TEAM_CODE   ·   Value: รหัสทีมที่ต้องการ  > Deploy
   (ไม่ตั้งก็ได้ แต่ใครมีลิงก์จะเห็นข้อมูลลูกค้าและยอดเงินทั้งหมด)
5) เปิดลิงก์ https://worker.<ชื่อบัญชี>.workers.dev แล้วกรอกรหัสทีม 1 ครั้ง

วิธีที่ 2: ใช้ Wrangler (ถ้ามี Node.js)
  npx wrangler login
  npx wrangler deploy
  npx wrangler secret put TEAM_CODE

หมายเหตุ
- ข้อมูลอัปเดตถึงกันภายในประมาณ 4 วินาที (หยุดดึงเมื่อพับหน้าจอไว้ เพื่อประหยัดโควตา)
- แพ็กเกจฟรี: Worker 100,000 ครั้ง/วัน · D1 อ่าน 5 ล้านแถว/วัน เขียน 100,000 แถว/วัน
- การอ่านใบนำฝากใช้การสแกนตัวอักษรในเบราว์เซอร์ (ไม่มี Claude อ่านรูปบนเว็บนี้)
- เมื่อมีการแก้ไขระบบ จะได้ไฟล์ worker.js ใหม่ ให้วางทับในขั้นตอนที่ 2 (ข้อมูลในฐานข้อมูลไม่หาย)

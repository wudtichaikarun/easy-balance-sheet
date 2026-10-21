# Easy Balance Sheet — Dashboards

Dashboard งบการเงินหุ้นอเมริกาแบบอ่านง่าย ตอบคำถามว่า "ถ้าบริษัทขายได้ $100 เหลือเท่าไร" ข้อมูลมาจาก 10-Q / 10-K บน SEC EDGAR

**ดูออนไลน์:** https://wudtichaikarun.github.io/easy-balance-sheet/

## เริ่มดู

เปิดลิงก์ด้านบน หรือเปิด `index.html` ในเครื่องได้เลย ไม่ต้องรัน server

- **จอคอม:** ด้านซ้ายเป็นรายชื่อหุ้น ด้านขวาแสดง dashboard ของหุ้นที่เลือก
- **มือถือ:** หน้าแรกเป็นรายการการ์ด แตะแล้วเปิดเต็มจอ
- ค้นหาได้ทั้ง ticker และชื่อธุรกิจ หุ้นที่มีหลายไตรมาสมีปุ่มสลับไตรมาส
- แต่ละหุ้นมีลิงก์ของตัวเอง เช่น `index.html#MU-2026Q4`
- ปุ่มเปลี่ยนธีมสว่าง/มืดอยู่มุมขวาบน

แต่ละ dashboard เปิดเดี่ยวๆ ได้เหมือนกัน

## ไฟล์ในโฟลเดอร์นี้

```
index.html                          # หน้ารวม สร้างจาก build_index.py ห้ามแก้เอง
<TICKER>-dashboard-<YYYY>Q<n>.html  # dashboard ของแต่ละหุ้น/ไตรมาส
```

ไตรมาสในชื่อไฟล์เป็นปีงบประมาณของบริษัท (fiscal year) จึงอาจไม่ตรงกับปีปฏิทิน เช่น `NVDA-dashboard-2027Q2.html`

## เพิ่มหรืออัปเดต dashboard

1. วางไฟล์ใหม่ในโฟลเดอร์นี้ ตั้งชื่อตามรูปแบบ `<TICKER>-dashboard-<YYYY>Q<n>.html`
2. รันจากโฟลเดอร์แม่: `uv run build_index.py`

ไฟล์ที่ชื่อไม่ตรงรูปแบบจะไม่ขึ้นใน index รายละเอียดการ build และการแก้หน้าตา index ดูที่ `README.md` ของ workspace (โฟลเดอร์แม่ ไม่ได้อยู่ใน repo นี้)

## Deploy

ทุกไฟล์เป็น static HTML อัปโหลดทั้งโฟลเดอร์นี้ขึ้น Netlify, Cloudflare Pages หรือ GitHub Pages ได้ทันที ไม่ต้องตั้งค่า build (แต่ต้องรัน `build_index.py` ในเครื่องก่อนทุกครั้ง)

### GitHub Pages (เหมาะถ้าจะอัปเดตบ่อย)

ต้องมี [gh CLI](https://cli.github.com/) และ login แล้ว รันในโฟลเดอร์นี้:

```bash
git init && git add . && git commit -m "dashboards"
gh repo create easy-balance-sheet --public --source=. --push
gh api repos/{owner}/easy-balance-sheet/pages -X POST -f "source[branch]=main" -f "source[path]=/"
```

เปลี่ยน `{owner}` เป็นชื่อบัญชี GitHub หรือเปิดเองใน Settings → Pages → Branch: `main` ก็ได้ รอ 1–2 นาทีจะได้ลิงก์ `https://<username>.github.io/easy-balance-sheet/`

มี dashboard ใหม่ก็แค่ build index แล้ว `git add . && git commit -m "update" && git push`

### เรื่องการมองเห็น

- ใครมีลิงก์ก็เปิดได้ ทั้ง GitHub Pages จาก repo public และลิงก์ Netlify
- GitHub Pages จาก private repo ต้องใช้แพ็กเกจเสียเงิน ถ้าบัญชีฟรีอยากซ่อน repo ให้ใช้ Netlify หรือ Cloudflare Pages ซึ่งเชื่อมกับ private repo ได้ฟรี

## ข้อควรรู้

- ไอคอนโหลดจาก CDN (Tabler Icons บน jsDelivr) ถ้าออฟไลน์ ไอคอนจะไม่ขึ้น แต่เนื้อหาอื่นใช้ได้ปกติ
- เนื้อหาเป็นการสรุปงบการเงินเพื่อการศึกษา ไม่ใช่คำแนะนำการลงทุน

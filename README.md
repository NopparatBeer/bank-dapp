# Bank DApp

เว็บธนาคารสำหรับงาน Smart Contract ของนพรัตน์ สวัสดิ์เอื้อ รหัสนักศึกษา 66050180 ใช้ MetaMask เพื่อฝาก ถอน และตรวจสอบยอดเงินจากสัญญา Bank บน Ethereum Hoodi

## ไฟล์ในโปรเจกต์

- `Bank.sol` คือ Smart Contract ที่ใช้ใน Remix IDE
- `index.html`, `styles.css` และ `app.js` คือหน้าเว็บและการเชื่อมต่อ MetaMask
- `config.js` เก็บเครือข่าย ที่อยู่สัญญา และข้อมูลนักศึกษาที่แสดงบนเว็บ

## Deploy สัญญาใน Remix

1. เปิด [Remix IDE](https://remix.ethereum.org/) สร้างไฟล์ `Bank.sol` แล้วคัดลอกโค้ดจากไฟล์ในโปรเจกต์
2. ไปที่ **Solidity Compiler** เลือก `0.8.20` แล้วกด **Compile Bank.sol**
3. ไปที่ **Deploy & Run Transactions** เลือก **Remix VM** และ `Bank` ตั้ง **Value** เป็น `0` แล้วกด **Deploy** เพื่อทดสอบในเครื่องจำลอง
4. หากต้องการทดสอบบน Hoodi ให้เลือก **Browser Extension** และ MetaMask ตรวจว่าเครือข่ายเป็น **Hoodi** และมี Hoodi ETH สำหรับค่า gas จากนั้นเลือก `Bank` ตั้ง **Value** เป็น `0` กด **Deploy** แล้วตรวจและยืนยันใน MetaMask
5. เมื่อธุรกรรมสำเร็จ ให้คัดลอก **Contract Address** จาก **Deployed Contracts** และเก็บ Transaction Hash จากประวัติ MetaMask เพื่อตรวจสอบบน [Hoodi Etherscan](https://hoodi.etherscan.io/)

ดูตำแหน่งเมนูและตัวเลือกเครือข่ายใน [คู่มือ Deploy & Run ของ Remix](https://remix-ide.readthedocs.io/en/latest/run.html)

## ที่อยู่สัญญา

เว็บตั้งค่า Bank Contract บน Hoodi ไว้ที่ [0x2c5564522d90bc97e2b580fef676543f793a1223](https://hoodi.etherscan.io/address/0x2c5564522d90bc97e2b580fef676543f793a1223) หาก Deploy สัญญาใหม่ ให้ใช้ขั้นตอนด้านล่างเพื่อเปลี่ยนที่อยู่

- ทดสอบบนเว็บได้ทันทีโดยวาง Contract Address ในช่อง **ที่อยู่สัญญา Bank** แล้วกด **บันทึก** ค่านี้จะอยู่ในเบราว์เซอร์ที่บันทึก
- ให้ผู้ตรวจเปิดเว็บแล้วเห็นสัญญาตั้งต้นได้โดยใส่ Contract Address จริงใน `contractAddress` ที่ไฟล์ `config.js` แล้วคอมมิตไฟล์ไปยังสาขาที่ GitHub Pages ใช้เผยแพร่

## ทดสอบจากหน้าเว็บ

1. เปิดหน้าเว็บผ่าน GitHub Pages ในเบราว์เซอร์ที่มี MetaMask แล้วกด **เชื่อมต่อ MetaMask**
2. เลือกบัญชีและอนุญาตให้เพิ่มหรือสลับไป **Hoodi** ใน MetaMask เมื่อมีคำขอ แล้วตรวจว่าช่อง Smart Contract แสดง **เชื่อมต่อแล้ว**
3. ใช้บัญชีที่ยังไม่เคยฝากกับสัญญานี้ กด **ตรวจสอบยอดเงิน** ควรเห็น `0 ETH` การอ่านยอดไม่ต้องยืนยันธุรกรรม
4. กรอก `0.001` ในช่องฝาก กด **ฝากเงิน** และยืนยันใน MetaMask เมื่อรายการสำเร็จ ยอดในสัญญาควรเป็น `0.001 ETH`
5. กรอก `0.0004` ในช่องถอน กด **ถอนเงิน** และยืนยัน เมื่อรายการสำเร็จ ยอดในสัญญาควรเป็น `0.0006 ETH`
6. ลองถอน `0.0007 ETH` ควรเห็นข้อความว่ายอดเงินไม่พอ สลับไปยังบัญชีใหม่ที่ยังไม่เคยฝากแล้วตรวจสอบว่ายอดเป็น `0 ETH`

ยอดที่แสดงคือเงินที่บัญชีปัจจุบันฝากไว้ในสัญญา ธุรกรรมฝากและถอนต้องใช้ ETH ในกระเป๋าสำหรับค่า gas ประวัติบนหน้าเว็บจะแสดงรายการที่ส่งตั้งแต่เปิดหน้านั้น

## ฟังก์ชันในสัญญา

| ฟังก์ชัน | การทำงาน |
| --- | --- |
| `deposit()` | เพิ่มยอดของผู้เรียกตาม ETH ที่แนบมากับธุรกรรม และต้องแนบค่ามากกว่าศูนย์ |
| `withdraw(uint256 amount)` | คืน ETH ให้ผู้เรียกตามจำนวนหน่วย wei ที่ขอ โดยจำนวนต้องมากกว่าศูนย์และไม่เกินยอดในสัญญา |
| `checkBlance()` | อ่านยอดของผู้เรียกในหน่วย wei โดยใช้ชื่อฟังก์ชันตามโจทย์ |

## GitHub Pages

ไฟล์เว็บอยู่ที่ราก repository และเปิดได้โดยตรงผ่าน GitHub Pages ตั้งค่า **Settings → Pages → Deploy from a branch** แล้วเลือก `main` และ `/(root)` หากใช้ชื่อสาขาอื่นให้เลือกสาขาที่มี `index.html` รอให้หน้า **Settings → Pages** แสดง URL ที่เผยแพร่แล้วจึงนำไปส่งใน dlearning

[คู่มือตั้งค่า GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)

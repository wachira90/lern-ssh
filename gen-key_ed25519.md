### การสร้าง SSH Key แบบ `ssh-ed25519` บน **Windows** และ **Linux**
---
## **1. บน Windows**
Windows มีหลายวิธีในการสร้าง SSH key แต่ที่นิยมคือการใช้ **PowerShell** หรือ **Git Bash** ซึ่งติดตั้ง `ssh-keygen` อยู่แล้ว

### **วิธีที่ 1: ใช้ PowerShell**
1. เปิด **PowerShell** (กด `Win + R` → พิมพ์ `powershell` → กด Enter)
2. ใช้คำสั่งนี้เพื่อสร้าง SSH key:
   ```powershell
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
   - `-t ed25519` → ระบุประเภทคีย์เป็น `ed25519`
   - `-C "your_email@example.com"` → คำอธิบายคีย์ (ใส่อีเมลของคุณหรือข้อมูลระบุอื่นๆ)
3. ระบบจะถามให้กำหนดชื่อไฟล์ (กด `Enter` เพื่อใช้ค่าเริ่มต้น: `C:\Users\YourUsername\.ssh\id_ed25519`)
4. ตั้งค่า **Passphrase** (หรือกด `Enter` หากไม่ต้องการรหัสผ่าน)
5. คีย์ถูกสร้างที่:
   ```
   C:\Users\YourUsername\.ssh\id_ed25519      (Private Key)
   C:\Users\YourUsername\.ssh\id_ed25519.pub  (Public Key)
   ```

### **วิธีที่ 2: ใช้ Git Bash**
1. เปิด **Git Bash** (ถ้ายังไม่มี ติดตั้งจาก [gitforwindows.org](https://gitforwindows.org/))
2. ใช้คำสั่งเดียวกับ PowerShell:
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
3. ไฟล์คีย์จะถูกเก็บไว้ใน `C:\Users\YourUsername\.ssh\`

---
## **2. บน Linux**
1. เปิด **Terminal**
2. พิมพ์คำสั่ง:
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
3. ตั้งชื่อไฟล์ (หรือกด Enter เพื่อใช้ค่าเริ่มต้น `~/.ssh/id_ed25519`)
4. ตั้งค่า Passphrase (กด Enter หากไม่ต้องการ)
5. คีย์จะถูกสร้างที่:
   ```
   ~/.ssh/id_ed25519      (Private Key)
   ~/.ssh/id_ed25519.pub  (Public Key)
   ```

---
## **3. เพิ่ม SSH Key ไปยัง SSH Agent (Windows/Linux)**
หลังจากสร้างคีย์แล้ว ควรเพิ่มไปยัง SSH agent เพื่อความสะดวกในการใช้งาน

```bash
eval "$(ssh-agent -s)"    # Start SSH agent
ssh-add ~/.ssh/id_ed25519 # เพิ่มคีย์ไปยัง SSH agent
```
> **Windows (PowerShell):** ถ้าใช้ `OpenSSH` ให้รัน `Start-Service ssh-agent` ก่อน

---
## **4. คัดลอก Public Key เพื่อใช้งาน**
ใช้คำสั่งนี้เพื่อดู Public Key และนำไปใช้กับ GitHub, GitLab, หรือเซิร์ฟเวอร์
```bash
cat ~/.ssh/id_ed25519.pub
```
หรือบน Windows (PowerShell):
```powershell
Get-Content C:\Users\YourUsername\.ssh\id_ed25519.pub
```
จากนั้นคัดลอกคีย์ที่ได้ไปใส่ใน **SSH Settings** ของแพลตฟอร์มที่คุณต้องการ

---
## 🎯 **สรุป**
| ระบบปฏิบัติการ | คำสั่งสร้าง SSH Key |
|---------------|---------------------|
| **Windows (PowerShell/Git Bash)** | `ssh-keygen -t ed25519 -C "your_email@example.com"` |
| **Linux/macOS** | `ssh-keygen -t ed25519 -C "your_email@example.com"` |


นำเนื้อหาในไฟล์ หรือ ไฟล์ "id_ed25519.pub" ส่งให้กับ admin เพื่อ add เข้าระบบ


### **วิธีแปลงคีย์ SSH `"id_ed25519"` เป็นคีย์สาธารณะ `*.ppk` บน Windows**

#### **วิธีที่ 1: ใช้ PuTTYgen (GUI)**
1. เปิด **PuTTYgen** (หากยังไม่มี สามารถดาวน์โหลดได้จาก [เว็บไซต์ PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html))
2. คลิก **"Load"** แล้วเลือกไฟล์คีย์ส่วนตัว `id_ed25519` ของคุณ  
   - ไฟล์อยู่ที่: `C:\Users\your_username\.ssh\id_ed25519`  
   - ในช่อง **"Files of type"** ให้เลือก **"All Files (*.*)"** เพื่อให้เห็นไฟล์ที่ไม่มีนามสกุล
3. หากมีการตั้งรหัสผ่าน (passphrase) ระบบจะถาม ให้กรอกรหัสผ่านของคีย์
4. คลิก **"Save private key"** เพื่อบันทึกเป็นไฟล์ `.ppk`
5. **(ตัวเลือกเสริม)** หากต้องการบันทึกคีย์สาธารณะ ให้ไปที่ **"Conversions" > "Export OpenSSH key"** แล้วบันทึกไฟล์ที่ได้

---

#### **วิธีที่ 2: ใช้คำสั่งผ่าน Command Line (`puttygen`)**
หากต้องการแปลงผ่าน Command Line สามารถใช้ `puttygen.exe` ซึ่งเป็นเครื่องมือของ PuTTY

1. เปิด **Command Prompt (cmd)**
2. ไปยังโฟลเดอร์ที่เก็บคีย์ SSH (`id_ed25519`):
   ```sh
   cd C:\Users\your_username\.ssh
   ```
3. ใช้คำสั่งนี้เพื่อแปลงคีย์เป็น `.ppk`:
   ```sh
   puttygen id_ed25519 -o id_ed25519.ppk
   ```
   - หากคีย์มีรหัสผ่าน ระบบจะให้กรอก passphrase

4. หากต้องการแปลงเป็น **คีย์สาธารณะที่ใช้กับ PuTTY**, ให้ใช้คำสั่ง:
   ```sh
   puttygen id_ed25519 -o id_ed25519.pub -O public
   ```

---

### **หมายเหตุ**
- ตรวจสอบว่า **puttygen.exe** ติดตั้งแล้วและอยู่ใน `PATH` ของระบบ หากยังไม่มี สามารถดาวน์โหลดจาก [เว็บไซต์ PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- หากคีย์ `id_ed25519` ไม่สามารถใช้งานกับ PuTTY ได้ อาจต้องแปลงเป็นคีย์ RSA ก่อน

หากมีข้อสงสัยเพิ่มเติม แจ้งได้เลยครับ! 🚀

To convert an OpenSSH private key (`id_ed25519`) to a PuTTY `.ppk` format on Windows, follow these steps:

### **Method 1: Using PuTTYgen (GUI)**
1. Open **PuTTYgen** (If you don’t have it, download it from [PuTTY website](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)).
2. Click on **"Load"** and select your `id_ed25519` private key (`C:\Users\your_username\.ssh\id_ed25519`).
   - Ensure "All Files (*.*)" is selected in the file type dropdown, as `id_ed25519` has no extension.
3. If prompted, enter the passphrase for your key (if set).
4. Click **"Save private key"** to save it as a `.ppk` file.
5. (Optional) To get the public key, go to **"Conversions" > "Export OpenSSH key"** and save it.

---

### **Method 2: Using Command Line (`puttygen`)**
If you prefer the command line, use `puttygen.exe` (part of PuTTY tools).

1. Open **Command Prompt (cmd)**.
2. Navigate to the directory where `id_ed25519` is stored:
   ```sh
   cd C:\Users\your_username\.ssh
   ```
3. Run the following command to convert it to `.ppk`:
   ```sh
   puttygen id_ed25519 -o id_ed25519.ppk
   ```
   - If your key is password-protected, it will prompt you to enter the passphrase.

4. If you need to convert it to a **PuTTY-compatible public key**, use:
   ```sh
   puttygen id_ed25519 -o id_ed25519.pub -O public
   ```

---

### **Additional Notes:**
- Ensure `puttygen.exe` is installed and added to your system `PATH`. If not, download it from the [PuTTY website](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).
- If the key format is incompatible, you may need to first convert `id_ed25519` to an RSA key.

Let me know if you need more help! 🚀



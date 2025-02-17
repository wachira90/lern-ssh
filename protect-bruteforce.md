จาก Log ที่ให้มา แสดงให้เห็นว่ามีความพยายามใช้ **Brute Force Attack** เพื่อพยายามเข้าสู่ SSH โดยการสุ่มชื่อผู้ใช้และรหัสผ่าน ดังนั้น ควรตั้งค่า SSH เพื่อป้องกันการโจมตีประเภทนี้ โดยใช้แนวทางดังต่อไปนี้:

---

## 🔒 **1. ปิดการเข้าถึง SSH ด้วยรหัสผ่าน ใช้ SSH Key Authentication แทน**
การตั้งค่าให้ SSH ยอมรับเฉพาะการล็อกอินด้วย SSH Key เท่านั้น จะช่วยป้องกันการโจมตีแบบ brute force ได้อย่างมีประสิทธิภาพ

### **วิธีการตั้งค่า**
1. **เปิดไฟล์การตั้งค่า SSH**
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. **แก้ไขการตั้งค่าต่อไปนี้:**
   ```bash
   PasswordAuthentication no
   PubkeyAuthentication yes
   ```
3. **บังคับให้ใช้ SSH Key เท่านั้น**
   ```bash
   ChallengeResponseAuthentication no
   ```
4. **บันทึกไฟล์ และรีสตาร์ท SSH**
   ```bash
   sudo systemctl restart sshd
   ```

---

## 🚫 **2. จำกัดการเข้าถึง SSH ตาม IP (Whitelist IP)**
หากเป็นไปได้ ควรอนุญาตให้เฉพาะ IP ที่ระบุสามารถเข้าถึง SSH ได้ เช่น เฉพาะ subnet ภายในองค์กร หรือ VPN เท่านั้น

### **วิธีการตั้งค่า**
1. **แก้ไขไฟล์การตั้งค่า SSH**
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. **เพิ่มบรรทัดนี้เพื่อกำหนดช่วง IP ที่สามารถเข้าถึงได้**
   ```bash
   AllowUsers user@192.168.1.*
   ```
   หรือใช้:
   ```bash
   AllowUsers user@10.0.0.*
   ```
3. **บันทึกไฟล์ และรีสตาร์ท SSH**
   ```bash
   sudo systemctl restart sshd
   ```

---

## ⏳ **3. จำกัดจำนวนการพยายามล็อกอินผิดพลาด**
เพื่อลดโอกาสที่แฮกเกอร์จะใช้ brute force ควรตั้งค่าจำกัดจำนวนครั้งที่สามารถป้อนรหัสผ่านผิดพลาด

### **วิธีการตั้งค่า**
1. **เปิดไฟล์ `sshd_config`**
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. **แก้ไขค่าต่อไปนี้:**
   ```bash
   MaxAuthTries 3
   MaxSessions 2
   ```
   - `MaxAuthTries 3` → กำหนดให้ป้อนรหัสผ่านผิดพลาดได้ไม่เกิน 3 ครั้งก่อนถูกตัดการเชื่อมต่อ
   - `MaxSessions 2` → จำกัดจำนวนเซสชัน SSH ที่เชื่อมต่อพร้อมกัน

3. **บันทึกไฟล์ และรีสตาร์ท SSH**
   ```bash
   sudo systemctl restart sshd
   ```

---

## 🔥 **4. ใช้ Fail2Ban เพื่อบล็อก IP ที่พยายามล็อกอินผิดหลายครั้ง**
`fail2ban` เป็นเครื่องมือที่ช่วยบล็อก IP ของผู้ที่พยายามล็อกอินผิดหลายครั้งในช่วงเวลาสั้น ๆ

### **ติดตั้ง Fail2Ban**
```bash
sudo apt update
sudo apt install fail2ban -y
```

### **ตั้งค่า Fail2Ban**
1. **สร้างไฟล์กำหนดค่าใหม่**
   ```bash
   sudo nano /etc/fail2ban/jail.local
   ```
2. **เพิ่มการตั้งค่าสำหรับ SSH**
   ```ini
   [sshd]
   enabled = true
   port = ssh
   filter = sshd
   logpath = /var/log/auth.log
   maxretry = 3
   bantime = 3600
   findtime = 600
   ```
   - `maxretry = 3` → บล็อก IP ที่พยายามผิดเกิน 3 ครั้ง
   - `bantime = 3600` → บล็อก IP นาน 1 ชั่วโมง
   - `findtime = 600` → นับความผิดพลาดภายใน 10 นาที

3. **รีสตาร์ท Fail2Ban**
   ```bash
   sudo systemctl restart fail2ban
   sudo systemctl enable fail2ban
   ```

---

## 🛑 **5. เปลี่ยนพอร์ต SSH เป็นพอร์ตอื่น**
การใช้พอร์ตเริ่มต้น (`22`) เป็นที่รู้กันดีสำหรับแฮกเกอร์ ดังนั้นควรเปลี่ยนไปใช้พอร์ตอื่นเพื่อลดการสแกน

### **วิธีการตั้งค่า**
1. **เปิดไฟล์ `sshd_config`**
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. **เปลี่ยนพอร์ต SSH (เลือกพอร์ตที่ไม่ชนกับบริการอื่น เช่น 2222, 2022, 5022)**
   ```bash
   Port 2222
   ```
3. **บันทึกไฟล์ และรีสตาร์ท SSH**
   ```bash
   sudo systemctl restart sshd
   ```

> **หมายเหตุ:** หากมี Firewall เช่น `UFW` หรือ `iptables` ต้องเปิดพอร์ตใหม่ก่อนรีสตาร์ท SSH

```bash
sudo ufw allow 2222/tcp
sudo ufw reload
```

---

## ✅ **6. ใช้ Two-Factor Authentication (2FA)**
สามารถใช้ **Google Authenticator** หรือ **Duo Security** เพื่อเพิ่มชั้นความปลอดภัยให้ SSH

### **ติดตั้ง Google Authenticator**
```bash
sudo apt install libpam-google-authenticator -y
```

### **เปิดใช้งาน**
```bash
google-authenticator
```

### **ตั้งค่า PAM ให้รองรับ 2FA**
1. **แก้ไขไฟล์ PAM**
   ```bash
   sudo nano /etc/pam.d/sshd
   ```
2. **เพิ่มบรรทัดนี้**
   ```bash
   auth required pam_google_authenticator.so
   ```

3. **แก้ไข `sshd_config`**
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
   เปลี่ยน:
   ```bash
   ChallengeResponseAuthentication yes
   ```
4. **รีสตาร์ท SSH**
   ```bash
   sudo systemctl restart sshd
   ```

---

## 🔐 **7. ใช้ Firewall เพื่อบล็อกการโจมตี**
สามารถใช้ `UFW` หรือ `iptables` เพื่อจำกัดการเข้าถึง SSH

### **ใช้ UFW**
```bash
sudo ufw allow 2222/tcp
sudo ufw enable
sudo ufw reload
```

### **ใช้ iptables**
```bash
sudo iptables -A INPUT -p tcp --dport 2222 -m state --state NEW -s 192.168.1.0/24 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 2222 -j DROP
```

---

## 🎯 **สรุป**
### ✅ ตั้งค่า SSH เพื่อป้องกัน Brute Force
- ❌ **ปิดการล็อกอินด้วยรหัสผ่าน** (`PasswordAuthentication no`)
- ✅ **ใช้ SSH Key Authentication เท่านั้น** (`PubkeyAuthentication yes`)
- 🌍 **จำกัดการเข้าถึงตาม IP** (`AllowUsers user@192.168.1.*`)
- 🔥 **ใช้ Fail2Ban เพื่อบล็อก IP ที่โจมตี**
- 🛑 **เปลี่ยนพอร์ต SSH**
- 🔑 **เปิดใช้งาน 2FA**
- 🚧 **ใช้ Firewall เพื่อป้องกันการสแกนพอร์ต**

หากทำทั้งหมดนี้แล้ว โอกาสที่จะถูกโจมตีด้วย Brute Force จะลดลงอย่างมาก 🎯

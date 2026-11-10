# Plan v3: Sửa slide bám sát cấu trúc báo cáo

> Trạng thái: **chờ anh/chị duyệt mục v3 bên dưới**. Em chưa sửa slide nào trong đợt này.
> File PDF hiện tại giống hệt bản em đọc lần trước (12 trang, so từng dòng không có khác biệt).

## v3.1 Vì sao slide hiện tại chưa khớp báo cáo

Nội dung chữ trên slide v2 phần lớn lấy từ báo cáo, nhưng em đã **tự sắp xếp và đặt tên lại**, nên khi đặt cạnh báo cáo thì khó đối chiếu:

1. **Tên mục khác báo cáo.** Ví dụ slide Nội dung ghi "01. Ảo hóa & vòng đời VM", báo cáo ghi "1. Hypervisor và vòng đời VM". Tương tự với "06. Quản lý tài nguyên" (báo cáo: "Linux doanh nghiệp: quản lý tài nguyên máy chủ"), "08. Tổng kết & Định hướng" (báo cáo: hai mục riêng "Tổng kết: các mảng kiến thức kết nối với nhau như thế nào" và "Định hướng sắp tới").
2. **Tiêu đề slide do em tự đặt**, không theo tiêu đề nhỏ của báo cáo. Ví dụ "IP & routing", "Mạng VM, RHEL 10 & DNS", "Docker", "4 nguyên tắc tư duy". Báo cáo dùng các tiêu đề "Routing và gateway", "Các chế độ mạng của VM", "Quản lý mạng trên RHEL 10", "DNS và phân giải tên", "Kiến trúc Docker"...
3. **Thứ tự ý bị đảo** để vừa bố cục. Ví dụ thứ tự kiểm tra mạng nằm trong mục "DNS và phân giải tên" của báo cáo, nhưng slide tách thành slide riêng; slide Storage dồn partition, LVM, filesystem vào một slide.
4. **Một vài chỗ em tự thêm**, không có trong báo cáo:
   - Bảng ánh xạ: cột Kubernetes của dòng "Registry, container" ghi "Điều phối container". Báo cáo chỉ ghi ECR, ECS, EKS cho AWS.
   - Nhãn tự đặt như "Máy chậm", "Đã nắm / Còn yếu", "Bảo vệ VM", "Web không vào được?" (ý có trong báo cáo nhưng chữ là của em).
   - Cột "Vai trò" của `.socket` ghi "Socket" (báo cáo không giải thích).

## v3.2 Cách sửa đề xuất

- **Mỗi slide = một tiêu đề nhỏ của báo cáo** (hoặc 2 tiêu đề liền nhau nếu ngắn), giữ đúng tên và thứ tự như báo cáo.
- **Nội dung là tóm tắt từng đoạn của báo cáo, theo đúng thứ tự đoạn**, mỗi ý 1 dòng ngắn. Không thêm nhãn, ví von hay ý không có trong báo cáo.
- **Ví dụ lệnh** chép đúng từ báo cáo, giữ nhãn "Ví dụ".
- **Slide Nội dung** dùng đúng tên 7 mục + "Tổng kết" + "Định hướng sắp tới" của báo cáo.
- Giữ thiết kế, ảnh, hiệu ứng, Q&A, Cảm ơn như cũ.

## v3.3 Dàn ý mới (theo đúng cấu trúc báo cáo)

| # | Tiêu đề slide (= tiêu đề trong báo cáo) | Tóm tắt nội dung |
|---|---|---|
| 1 | Cover | Như cũ |
| 2 | Nội dung | Mở đầu · 1. Hypervisor và vòng đời VM · 2. Network · 3. Storage · 4. User, group và permission · 5. Service và firewall · 6. Quản lý tài nguyên máy chủ · 7. Container · Tổng kết · Định hướng sắp tới |
| 3 | Mở đầu | Phạm vi (cài RHEL 10.2 + Docker để làm quen, tập trung lý thuyết, lệnh là ví dụ) · Vì sao chọn RHEL · RHEL 10 / 10.2, minor 6 tháng · subscription, BaseOS / AppStream · Developer Subscription 16 hệ thống · Sắp xếp theo thứ tự máy chủ cần có · Mỗi phần gồm khái niệm, cách hoạt động, điểm riêng RHEL 10, ví dụ |
| 4 | 1. Hypervisor là gì · Hai loại hypervisor | Chia tài nguyên thành VM, vCPU, VT-x / AMD-V · ví dụ 32 CPU / 128 GB → 10–15 VM · Type 1 / Type 2 · hai điểm thú vị: Hyper-V là Type 1; RHEL làm hypervisor (KVM + QEMU + libvirt), Nitro dựa trên KVM |
| 5 | 1. Một VM gồm những gì · Vòng đời của một VM | Nhóm file, định dạng đĩa, thin / thick · overcommit · RHEL 10 cần x86-64-v3, lỗi glibc · 5 giai đoạn, hủy subscription · template, clone làm sạch machine-id / SSH host key / hostname · live migration |
| 6 | 1. Snapshot khác backup ở điểm nào | File chênh lệch, nằm cạnh VM · backup độc lập · Undo / két sắt · 3-2-1 · ví dụ `dnf update` |
| 7 | 2. Network · Routing và gateway | CIDR /24, .0 / .255 / 254 · 3 dải private · ARP, gateway · prefix dài nhất, default route, metric · ví dụ `ip route` và cách đọc |
| 8 | 2. Các chế độ mạng của VM · Quản lý mạng trên RHEL 10 | NAT, Host-only, Bridged, Internal; Hyper-V External / Internal / Private; bố trí 2 card · NetworkManager, connection profile, bỏ ifcfg, keyfile `.nmconnection`, nmcli / nmtui / Cockpit · ví dụ nmcli |
| 9 | 2. DNS và phân giải tên | nsswitch.conf, resolv.conf do NM sinh · A, AAAA, CNAME, PTR, MX · thứ tự kiểm tra khi mất kết nối (6 bước) · ví dụ 8.8.8.8 / google.com |
| 10 | 3. Storage · Ổ đĩa và partition · LVM | Không chung phân vùng với OS · các lớp · tên block device · MBR / GPT, fdisk / gdisk / parted · PV / VG / LV, extent 4 MiB · VG "rhel" · LVM devices file |
| 11 | 3. Filesystem và mount | XFS, ext4, Btrfs · mount, fstab 6 trường, UUID, emergency mode, `mount -a` · ví dụ pvcreate … lvextend · swap |
| 12 | 4. User, group và permission | UID 0 / <1000 / ≥1000 · passwd / shadow / group · nhóm chính, nhóm phụ · RHEL 10 khóa root, wheel + sudo |
| 13 | 4. Quyền truy cập file · sudo và least privilege | u / g / o, r w x, ý nghĩa với thư mục, 750 · umask 022 · setuid / setgid / sticky · ACL · ví dụ An & Bình · dòng sudoers, visudo |
| 14 | 5. Service (systemd) | PID 1 · loại unit, target · start / enable · `/usr/lib` vs `/etc` · daemon-reload · journald · Restart=, User= · ví dụ 3 lệnh |
| 15 | 5. Firewall với firewalld · SELinux | nftables, zone public, runtime / permanent · ví dụ · enforcing, nhãn, httpd_t / httpd_sys_content_t, lỗi 403, semanage + restorecon, không tắt SELinux, log AVC · 4 nguyên nhân "web không vào được" |
| 16 | 6. CPU và load average · RAM và swap | 4 câu hỏi · định nghĩa load, 3.95 / 2 CPU, xu hướng · top us / sy / wa / id / st · ví dụ `free -h`, available, swap, OOM killer |
| 17 | 6. Disk · Tiến trình · cgroups v2 và công cụ RHEL | Hết chỗ / nghẽn, df -h, du -sh, df -i, iostat · PID, SIGTERM / SIGKILL, nice · cgroups v2, ví dụ set-property · Cockpit, tuned, sos report |
| 18 | 7. Container khác VM ở điểm nào · Kiến trúc Docker | Tiến trình được cô lập · VM / container, 6 namespace, cgroups v2 · nhà riêng / chung cư · đánh đổi, chạy container trong VM · CLI → dockerd → containerd → runc, OCI, Podman, nhóm docker |
| 19 | 7. Image · Registry | Layer, lớp ghi, cache, lệnh Dockerfile, ví dụ Dockerfile, nguyên tắc tốt, UBI · Docker Hub / registry.redhat.io / Quay.io / ECR, tên đầy đủ, digest, build → tag → push → pull |
| 20 | 7. Network | 5 driver · bridge: namespace, veth, docker0 · sơ đồ 4 chặng 3 lần NAT (click từng chặng) · conntrack · publish port = DNAT · user-defined network, 127.0.0.11 · zone docker, `-p 127.0.0.1:8080:80` |
| 21 | 7. Volume · Resource limit | Named volume / bind mount / tmpfs, `:Z` / `:z` · mặc định không giới hạn, --memory / --cpus / --pids-limit, OOMKilled, restart policy, docker stats |
| 22 | Tổng kết: các mảng kiến thức kết nối với nhau như thế nào | Các lớp chồng lên nhau (sơ đồ) · 3 khái niệm xuyên suốt · 4 nguyên tắc tư duy · tự đánh giá |
| 23 | Định hướng sắp tới | GĐ1 – GĐ4 như báo cáo, GĐ3 gồm phần ánh xạ AWS (EC2 = VM, VPC…, NAT / Internet Gateway, Security Group, EBS / EFS / S3, IAM, EBS snapshot / AWS Backup, ECR / ECS / EKS, CloudWatch) |
| 24 | Q&A | Như cũ |
| 25 | Cảm ơn | Như cũ |

**Đã chốt (anh/chị trả lời):**
- Làm theo dàn ý trên.
- **Slide ánh xạ K8s / AWS: chỉ ghi tiêu đề**, nội dung anh/chị tự nói. Em đặt slide này sau "Định hướng sắp tới" (slide 24), nội dung ánh xạ trong báo cáo đưa vào presenter notes. Slide 23 chỉ tóm tắt GĐ1–GĐ4, không liệt kê ánh xạ.
- Tổng: **26 slide** (24 = ánh xạ, 25 = Q&A, 26 = Cảm ơn), khoảng 15–16 phút.
- Một số slide chuyển sang layout có thẻ bên phải để chứa 2 tiêu đề nhỏ, nên ảnh `laptop-code`, `ethernet`, `racks` không còn dùng. Ảnh dùng: cover (1), port-containers (21), thanks (26).

## v3.4 Các bước (chờ anh/chị cho phép)

1. Viết lại `slides.md` theo v3.3, notes theo đúng đoạn tương ứng trong báo cáo.
2. Xuất ảnh, kiểm tra tràn chữ, đối chiếu từng slide với báo cáo; cập nhật `previews/`.
   - ⏸ Điểm dừng: gửi ảnh để anh/chị duyệt.
3. Xuất lại PPTX (25 trang), cập nhật `HUONG_DAN.md` (số slide có click).
4. Push GitHub sau khi anh/chị đồng ý.

---

# Plan v2 (đã thực hiện): Cập nhật slide theo báo cáo mới

> Trạng thái: **đã thực hiện xong bước 1–4**, đang được thay bằng plan v3 ở trên.
> Nguồn nội dung: `Báo cáo quá trình học việc.pdf` **bản mới (12 trang)**. Bản cũ 10 trang không còn dùng.
> Nguyên tắc: em không tự chạy bước nào. Trước mỗi bước ở mục 8, em hỏi và chờ anh/chị đồng ý.
> Nguyên tắc nội dung: slide **chỉ dùng thông tin có trong báo cáo mới**, không thêm số liệu hay khẳng định ngoài báo cáo.

---

## 0. Các quyết định đã chốt

| Hạng mục | Quyết định |
|---|---|
| Số slide, thời lượng | **Phương án B: 23 slide, 15 phút** |
| Card "Bài học" | Đổi tên thành **"Lưu ý"**, nội dung chỉ lấy lưu ý có trong báo cáo |
| Câu "lệnh chỉ là ví dụ minh họa" | **Ghi rõ trên slide 3**, mỗi khối lệnh có nhãn "Ví dụ" |
| Sơ đồ đường đi gói tin (slide 17) | **Hiện từng chặng theo click** |
| Ảnh | **Giữ 8 ảnh hiện có**, không tải thêm |
| GitHub | **Push** lên https://github.com/leluc212/Present_Week_2 sau khi anh/chị duyệt (commit mới, không force push) |
| Giữ nguyên từ plan cũ | Thiết kế (màu, font Be Vietnam Pro, cỡ chữ đã tăng, 7 layout, component trang trí), tiếng Việt + thuật ngữ tiếng Anh, không logo, trình chiếu trên trình duyệt, PPTX là bản phụ, hiệu ứng vừa phải, slide Q&A chỉ có chữ "Q&A", giữ slide ánh xạ K8s / AWS |

---

## 1. Báo cáo mới khác bản cũ ở đâu

### 1.1 Thay đổi về phạm vi (ảnh hưởng toàn bộ slide)

| Bản cũ | Bản mới |
|---|---|
| Thực hành trên **Rocky Linux**, có lab | Cài **RHEL 10.2** và Docker **để làm quen**. Thời gian thực hành còn ít |
| Lệnh là những gì "em đã chạy" | Lệnh **chỉ là ví dụ minh họa**, không phải bài lab đã làm hoàn chỉnh |
| Mỗi phần có "bài học" từ lỗi em gặp | Mỗi phần gồm: khái niệm, cách hoạt động, điểm riêng của RHEL 10, ví dụ minh họa |
| Có "lab xuyên suốt" web01 / db01 | **Không còn lab xuyên suốt** |

→ Trên slide phải bỏ mọi câu kể việc đã làm ("em đã thử", "em gặp lỗi", "lúc đầu em...").

### 1.2 Nội dung bị bỏ (đang có trên slide, phải xóa)

- Rocky Linux; câu "thực hành trên Rocky Linux và Docker, hypervisor mới lý thuyết"
- Proxmox trong danh sách Type 1
- Lỗi "khai báo gateway cho cả hai card nên mất Internet" (kể như trải nghiệm cá nhân). Bản mới chỉ nói chung: thường chỉ card ra Internet mới khai báo gateway
- Lệnh `lsblk`, `fdisk /dev/sdb`, `blkid`; ví dụ gắn ổ ảo 10 GB vào web01
- `useradd -m -G webteam an/binh`, lỗi `usermod -G wheel` thiếu `-a`
- Unit file `hello-app` tự viết, thử `kill -9` cho systemd tự chạy lại
- Tình huống tự đặt "web01 chạy chậm", giả lập tải bằng `yes`, `dd`; `nproc`, `uptime`, `renice -n 10`, `kill -15 2345`
- Registry nội bộ (web01 push, db01 pull), `insecure-registries`, `docker history`
- Thử xóa container PostgreSQL để kiểm chứng volume
- Toàn bộ slide "Lab xuyên suốt: web hai tầng trên 2 VM"
- "4 bài học" bản cũ, "Tự đánh giá" bản cũ, lộ trình giai đoạn 1 bản cũ
- K8s "trên 3 VM Rocky Linux"; RHCSA "nâng lab lên RHEL 10, đào sâu SELinux"

### 1.3 Nội dung mới (phải thêm)

| Phần | Nội dung mới |
|---|---|
| Mở đầu | Lý do chọn RHEL; RHEL 10 ra mắt 5/2025, bản 10.2 (minor release) 5/2026, minor khoảng 6 tháng một lần, tương thích trong dòng 10; phải đăng ký **subscription** mới dùng được kho phần mềm; 2 repo **BaseOS** (lõi OS) và **AppStream** (ứng dụng, ngôn ngữ, công cụ); **Developer Subscription for Individuals** miễn phí, tối đa **16 hệ thống** |
| 01 Hypervisor | RHEL cũng làm được hypervisor: **KVM** trong kernel + **QEMU** giả lập phần cứng + **libvirt** quản lý; Nitro của EC2 dựa trên KVM |
| 01 VM | RHEL 10 yêu cầu CPU **x86-64-v3** (AVX2, từ Intel Haswell / AMD Excavator); hypervisor phải chuyển tính năng CPU vào VM, nếu không RHEL 10 không khởi động và báo **lỗi glibc** |
| 01 Vòng đời | Hủy đăng ký subscription trước khi xóa VM; clone phải làm sạch **machine-id, SSH host key, hostname** |
| 01 Snapshot | Ví dụ: snapshot trước khi chạy `dnf update` |
| 02 Network | CIDR /24 = 256 địa chỉ (.0 mạng, .255 broadcast, 254 dùng được); 3 dải private 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16; cùng mạng dùng **ARP** tìm MAC; chọn route **prefix dài nhất**, không khớp thì đi default route; nhiều route cùng đích thì **metric** nhỏ hơn được ưu tiên |
| 02 RHEL 10 | NetworkManager, connection profile gắn với device; **RHEL 10 bỏ hẳn ifcfg** trong `/etc/sysconfig/network-scripts/`, profile lưu dạng **keyfile `.nmconnection`** trong `/etc/NetworkManager/system-connections/`; cấu hình bằng nmcli, nmtui hoặc Cockpit |
| 02 DNS | Thứ tự tra tên trong `/etc/nsswitch.conf` (hosts trước, DNS sau); `/etc/resolv.conf` do NetworkManager sinh, sửa tay bị ghi đè; bản ghi A, AAAA, CNAME, PTR, MX |
| 02 Kiểm tra | Thêm bước kiểm tra cổng dịch vụ (`ss`, `curl`); ví dụ ping 8.8.8.8 được mà ping google.com không được thì lỗi ở DNS |
| 03 Storage | Các lớp: ổ đĩa → partition → (tùy chọn) LVM → filesystem → mount; tên thiết bị `/dev/sda`, `/dev/nvme0n1`, `/dev/vda`; **MBR** (tối đa 2 TB, 4 partition chính) và **GPT** (ổ rất lớn, mặc định 128 partition, đi với UEFI); fdisk, gdisk, parted; **extent** mặc định 4 MiB; cài tự động thì OS nằm trên LVM, VG tên **"rhel"** với LV root và swap; **LVM devices file** `/etc/lvm/devices/system.devices` (từ RHEL 9) |
| 03 Filesystem | XFS mặc định, chỉ mở rộng; **ext4** vẫn hỗ trợ (thu nhỏ được khi đã umount); **Btrfs không được hỗ trợ**; fstab có **6 trường**; **swap** là "RAM dự phòng" nhưng chậm hơn RAM nhiều |
| 04 User | **UID**: root 0, tài khoản hệ thống dưới 1000, user thường từ 1000; 3 file passwd / shadow / group; **nhóm chính** (tự tạo trùng tên) và **nhóm phụ**; **RHEL 10 mặc định khóa root**, khuyến khích user quản trị thuộc nhóm wheel dùng sudo |
| 04 Permission | Ý nghĩa rwx với thư mục; **umask** 022 → file 644, thư mục 755; 3 quyền đặc biệt **setuid** (ví dụ passwd), **setgid**, **sticky bit** (`/tmp` 1777); **ACL** (setfacl, getfacl); luôn sửa sudoers bằng **visudo** |
| 05 systemd | Các loại unit: .service, .socket, .timer (thay cron), .mount (sinh từ fstab), .target (thay runlevel); multi-user.target, graphical.target; unit file của gói ở `/usr/lib/systemd/system/`, của quản trị viên ở `/etc/systemd/system/` (được ưu tiên); `daemon-reload`; journald |
| 05 Firewall | firewalld dùng **nftables**; zone mặc định **public**; **runtime** (hiệu lực ngay, mất khi reload/reboot) và **permanent** (chỉ hiệu lực sau reload) |
| 05 SELinux | Mặc định **enforcing**; mọi tiến trình và file có nhãn (context); ví dụ `httpd_t` chỉ đọc file `httpd_sys_content_t`; xử lý bằng semanage + restorecon, **không tắt SELinux**; log **AVC** |
| 06 Tài nguyên | 4 câu hỏi CPU / RAM / disk / tiến trình; định nghĩa load average (1, 5, 15 phút, gồm cả chờ I/O không ngắt được), so 3 số để thấy xu hướng; các chỉ số top **us, sy, wa, id, st** (st liên quan overcommit); **OOM killer**; hết dung lượng khác nghẽn tốc độ; **inode** (`df -i`); `iostat` |
| 06 Tiến trình | **SIGTERM (15)** thử trước, **SIGKILL (9)** khi không phản hồi; **nice** từ -20 đến 19, mặc định 0; user thường chỉ được giảm ưu tiên |
| 06 RHEL | **RHEL 10 chỉ còn cgroups v2**; **Cockpit** (cổng 9090), **tuned** (profile virtual-guest), **sos report** |
| 07 Container | Container là **tiến trình Linux được cô lập**, không phải "máy ảo nhỏ"; vài chục đến vài trăm MB; 6 namespace pid, net, mnt, uts, ipc, user; cô lập kém VM vì chung kernel; doanh nghiệp thường chạy container trong VM |
| 07 Docker | Kiến trúc **docker CLI → dockerd → containerd → runc**; chuẩn **OCI**; trên RHEL công cụ mặc định là **Podman** (không daemon, chạy được không cần root), Docker cài từ repo của Docker |
| 07 Image | Mỗi lệnh Dockerfile tạo 1 layer, layer dùng chung; lớp ghi mỏng trên cùng; thứ tự bước để tận dụng cache; các lệnh FROM, RUN, COPY, ENV, EXPOSE, USER, CMD/ENTRYPOINT; nguyên tắc tốt (image nhỏ, gắn phiên bản, không chạy root, không đưa mật khẩu vào image); **UBI** của Red Hat |
| 07 Registry | Docker Hub, registry.redhat.io, Quay.io, Amazon ECR, registry nội bộ; tên đầy đủ `registry/namespace/tên:tag`; **digest** sha256; quy trình build → tag → push → pull |
| 07 Volume | 3 cách: **named volume** (`/var/lib/docker/volumes/`), **bind mount**, **tmpfs**; `:Z` (riêng 1 container) và `:z` (dùng chung) |
| 07 Network | 5 driver bridge, host, none, overlay, macvlan; cơ chế bridge: network namespace, cặp **veth**, **docker0** 172.17.0.1 làm gateway; **sơ đồ 4 chặng, 3 lần NAT** từ container ra Internet (MASQUERADE → NAT hypervisor → NAT router, thêm CGNAT nếu có); `ip_forward`; **conntrack**; publish port là rule **DNAT**; DNS nội bộ **127.0.0.11** trong user-defined network; Docker tạo **zone docker** trong firewalld; giới hạn IP lắng nghe `-p 127.0.0.1:8080:80` |
| 07 Limit | Mặc định không giới hạn; `--memory`, `--cpus`, `--pids-limit`; restart policy `--restart on-failure`; `docker stats` |
| Tổng kết | 7 mảng là **các lớp chồng lên nhau**; **3 khái niệm xuyên suốt**: cgroups, routing & NAT, phân quyền nhiều lớp; **4 nguyên tắc tư duy** (bản mới, khác bản cũ) |
| Tự đánh giá | Nắm lý thuyết và hiểu "vì sao", nhưng **kinh nghiệm thực hành còn rất ít**; ưu tiên số một là biến lý thuyết thành kỹ năng |
| Định hướng | GĐ1 mới: tự thực hành lại toàn bộ báo cáo trên RHEL 10.2 (danh sách cụ thể), ghi chép lỗi, viết script Bash; GĐ2: kubeadm trên 3 VM (không còn ghi Rocky); GĐ3: thêm **NAT Gateway, Internet Gateway**; GĐ4: RHCSA buộc luyện tay nhiều, học thêm Podman, sau đó RHCE |

---

## 2. Dàn ý 23 slide (phương án B)

Ký hiệu cột "So với bản cũ": **Giữ** (giữ bố cục, sửa nhẹ) · **Sửa** (viết lại nội dung) · **Mới** · (số slide cũ).
Slide dày sẽ dùng lưới thẻ nhỏ thay cho gạch đầu dòng dài. Chi tiết không vừa slide chuyển vào presenter notes.

| # | Slide | Nội dung chính trên slide | Layout | Ảnh | Hiệu ứng | So với bản cũ |
|---|---|---|---|---|---|---|
| 1 | Cover | Tiêu đề, pill "Linux doanh nghiệp · Ảo hóa · Container", Lê Tấn Lực (Intern), 11/2026 | cover | cover-main, cover-small | – | Giữ (1) |
| 2 | Nội dung | 8 mục như cũ | agenda | – | – | Giữ (2) |
| 3 | Cách tiếp cận & RHEL 10.2 | **Trái:** 7 bước vòng đời máy chủ · Phạm vi: cài RHEL 10.2 + Docker để làm quen, báo cáo tập trung lý thuyết, **lệnh trong slide là ví dụ minh họa**. **Thẻ phải:** vì sao chọn RHEL · RHEL 10 (5/2025), 10.2 minor (5/2026), ~6 tháng một bản, tương thích trong dòng 10 · subscription · BaseOS / AppStream · Developer Subscription miễn phí, 16 hệ thống | title-left-card | – | `v-clicks` 7 bước | Sửa (3), gộp nội dung mới |
| 4 | 01 · Hypervisor | Định nghĩa, VT-x / AMD-V · Type 1 (ESXi, Hyper-V, KVM) / Type 2 (VirtualBox, Workstation) · Hyper-V là Type 1 · RHEL làm hypervisor: KVM + QEMU + libvirt · Nitro dựa trên KVM | title-left-card | – | viền cam "Type 1" | Sửa (4) |
| 5 | 01 · VM & vòng đời | VM = file cấu hình + đĩa ảo (VDI, VHDX, VMDK, QCOW2; thin / thick) · Overcommit · 5 giai đoạn · Template / clone làm sạch machine-id, SSH host key, hostname · Hủy subscription trước khi xóa · Live migration · **Lưu ý RHEL 10:** CPU x86-64-v3, thiếu thì lỗi glibc | title-right | laptop-code | – | Sửa (5) |
| 6 | 01 · Snapshot ≠ Backup | Snapshot (Undo, file chênh lệch, nằm cạnh VM) · Backup (đầy đủ, độc lập, nơi khác) · 3-2-1 · Ví dụ: snapshot trước `dnf update`, xóa snapshot khi cập nhật thành công, backup vẫn chạy hằng đêm | side-panel | – | `v-click` cột Backup | Sửa (6) |
| 7 | 02 · IP & routing | CIDR /24 (256 địa chỉ, 254 dùng được) · 3 dải private · Cùng mạng: ARP · Khác mạng: gateway; prefix dài nhất, default route, metric · Ví dụ `ip route` (2 dòng, có metric) · **Lưu ý:** chỉ card ra Internet khai báo gateway | title-left-card | – | – | Sửa (7) |
| 8 | 02 · Mạng VM, RHEL 10 & DNS | 4 chế độ mạng VM (+ External / Internal / Private switch trên Hyper-V), bố trí 2 card · NetworkManager: profile gắn device, **bỏ ifcfg**, keyfile `.nmconnection`; nmcli / nmtui / Cockpit · Ví dụ nmcli đặt IP + DNS · nsswitch, resolv.conf do NM sinh, bản ghi A / AAAA / CNAME / PTR / MX | title-left-card | – | – | **Mới** (tách từ 7) |
| 9 | 02 · Kiểm tra mạng theo lớp | ip addr → ip route → ping gateway → ping IP đích → DNS → cổng dịch vụ (ss, curl) · Ví dụ: ping 8.8.8.8 được, ping google.com không được → lỗi DNS | title-right | ethernet | `v-clicks` 6 bậc | Sửa (8) |
| 10 | 03 · Storage | **Sơ đồ trái:** ổ đĩa → partition → PV → VG (extent 4 MiB) → LV → XFS → mount. **Phải:** không chung phân vùng với OS · tên thiết bị · MBR vs GPT · VG "rhel" mặc định · LVM devices file · XFS chỉ mở rộng, ext4, Btrfs không hỗ trợ · fstab 6 trường + UUID · swap · **Lưu ý:** sai fstab → emergency mode, `mount -a` trước reboot | title-right (sơ đồ trái) | – | – | Sửa (9) |
| 11 | 04 · User, group & permission | **Trái:** UID 0 / <1000 / ≥1000 · passwd / shadow / group · nhóm chính / phụ · RHEL 10 khóa root, wheel + sudo · rwx = 4/2/1, ý nghĩa với thư mục, 750 · umask 022 · setuid / setgid / sticky · ACL. **Thẻ phải:** Ví dụ An & Bình (`groupadd`, `usermod -aG`, `chown`, `chmod 2770`) · dòng sudoers least privilege · **Lưu ý:** sửa sudoers bằng visudo | title-left-card | – | viền cam `-a` | Sửa (10) |
| 12 | 05 · systemd | **Trái:** bảng loại unit (.service, .socket, .timer, .mount, .target) + multi-user / graphical target · 3 lệnh ví dụ. **Phải:** PID 1 · start ≠ enable, `enable --now` · `/usr/lib/...` vs `/etc/systemd/system/` (ưu tiên), `daemon-reload` · journald · `Restart=on-failure`, `User=` | title-right (bảng trái) | – | – | Sửa (11) |
| 13 | 05 · Firewall & SELinux | **Thẻ trái:** 4 nguyên nhân "web không vào được". **Phải:** firewalld + nftables, zone public, runtime vs permanent · ví dụ `--permanent` + `--reload` · SELinux enforcing, nhãn httpd_t / httpd_sys_content_t, lỗi 403, semanage + restorecon · **Lưu ý:** không tắt SELinux, xem log AVC | side-panel | – | – | Sửa (12) |
| 14 | 06 · Quản lý tài nguyên | **Trái:** 4 câu hỏi · load average (định nghĩa, xu hướng) · top: us, sy, wa, id, st · RAM: available, swap tăng, OOM killer · Disk: df -h, du -sh, df -i (inode), iostat · SIGTERM / SIGKILL, nice -20…19 · cgroups v2, ví dụ `set-property`. **Thẻ phải:** 3.95 trên 2 CPU · `free -h` · Cockpit 9090, tuned, sos report | title-left-card | – | – | Sửa (13) |
| 15 | 07 · Container vs VM | Container là tiến trình Linux được cô lập · VM = nhà riêng, container = căn hộ chung cư · 6 namespace + cgroups v2 · vài chục–vài trăm MB, vài giây · Đánh đổi: cô lập kém hơn, cần kernel Linux, nên chạy container trong VM | title-right | port-containers | `v-click` thẻ Container | Sửa (14) |
| 16 | 07 · Docker | Lưới 6 thẻ: **Kiến trúc** (CLI → dockerd → containerd → runc, OCI, Podman mặc định trên RHEL) · **Image** (layer, lớp ghi, cache, lệnh Dockerfile, UBI) · **Registry** (Docker Hub, registry.redhat.io, Quay.io, ECR; `registry/namespace/tên:tag`; digest; build → tag → push → pull) · **Volume** (named / bind / tmpfs, `:Z` / `:z`) · **Resource limit** (`--memory`, `--cpus`, `--pids-limit`, OOMKilled, `--restart on-failure`, `docker stats`) · **Lưu ý** (nhóm docker gần như root; image nhỏ, gắn phiên bản, không chạy root, không đưa mật khẩu vào image) | agenda (lưới thẻ) | – | – | Sửa (15) |
| 17 | 07 · Docker network | **Sơ đồ trái:** 4 chặng, 3 lần NAT, đúng IP trong báo cáo (container 172.17.0.2 → docker0 172.17.0.1 → MASQUERADE → VM web01 10.0.2.15 / 192.168.56.10 → NAT hypervisor → laptop 192.168.1.20 → NAT router → Internet). **Phải:** 5 driver · bridge: namespace, veth, docker0 · conntrack · publish port = DNAT · user-defined network, DNS 127.0.0.11 · **Lưu ý:** zone docker, port publish mở ra ngoài, `-p 127.0.0.1:8080:80` | title-right (sơ đồ trái) | – | `v-clicks` từng chặng | **Mới** |
| 18 | 08 · Các lớp kết nối với nhau | **Sơ đồ trái:** 7 lớp xếp chồng (hypervisor → network → storage → user → systemd + firewalld + SELinux → tài nguyên → container), ví dụ x86-64-v3. **Phải:** 3 khái niệm xuyên suốt: cgroups · routing & NAT · phân quyền nhiều lớp | title-right (sơ đồ trái) | – | – | **Mới** (thay slide lab 16) |
| 19 | 08 · 4 nguyên tắc & tự đánh giá | **Thẻ trái:** tự đánh giá (nắm lý thuyết, hiểu "vì sao"; thực hành còn rất ít, chưa thành thạo, chưa xử lý sự cố thật; ưu tiên số một: biến lý thuyết thành kỹ năng). **Phải:** 4 nguyên tắc: nhìn theo lớp · kiểm tra từ dưới lên (IP, route, gateway, DNS, cổng, ứng dụng) · đọc log trước (journalctl, systemctl status, log AVC) · luôn có đường lui (snapshot / backup, `mount -a`) | side-panel | – | `v-clicks` 4 nguyên tắc | Sửa (16, 17) |
| 20 | 08 · Định hướng | GĐ1 thực hành lại toàn bộ trên RHEL 10.2 (VM + snapshot / backup / clone, nmcli 2 VM, LVM, phân quyền, web + firewalld + SELinux, giả lập tải, Docker + kiểm chứng đường đi mạng), ghi chép lỗi, script Bash · GĐ2 Kubernetes kubeadm 3 VM (1 control plane, 2 worker) · GĐ3 AWS SAA-C03 (30 / 26 / 24 / 20%), Free Tier có cảnh báo chi phí · GĐ4 RHCSA EX200 (RHEL 10, thi thực hành, phải còn đúng sau reboot), Podman, rồi RHCE EX294 Ansible | title-left-card | – | – | Sửa (17) |
| 21 | Ánh xạ kiến thức → K8s / AWS | Bảng cũ, **thêm dòng "Routing & NAT → NAT Gateway, Internet Gateway"** | agenda | – | – | Sửa (18) |
| 22 | Q&A | Chỉ chữ "Q&A" | qa | – | – | Giữ (19) |
| 23 | Cảm ơn | Như cũ, câu xin góp ý | thank-you | thanks-main, thanks-small | – | Giữ (20) |

**Ảnh `racks.jpg`:** slide 3 đổi sang layout có thẻ bên phải để chứa thông tin RHEL 10.2, nên không còn chỗ cho ảnh này. File ảnh vẫn giữ trong `public/images/`, chỉ không dùng. Nếu anh/chị muốn giữ ảnh ở slide 3, em sẽ chuyển thông tin RHEL 10.2 vào presenter notes.

**Phân bổ thời gian (15 phút):** mở đầu slide 1–3 khoảng 1,5 phút · phần 01–07 (slide 4–17) khoảng 10,5 phút · tổng kết và định hướng (slide 18–21) khoảng 2,5 phút · Q&A, cảm ơn 0,5 phút.

---

## 3. Quy tắc viết lại nội dung

1. **Đối chiếu từng con số và tên lệnh với báo cáo mới** (IP, phần trăm, cổng 9090, 16 hệ thống, 4 MiB, 128 partition, 2 TB, UID 1000, umask 022, nice -20…19, 127.0.0.11...). Không giữ lại số nào từ bản cũ nếu bản mới không có.
2. **Không viết ở ngôi "em đã làm"**. Mỗi khối lệnh có nhãn "Ví dụ".
3. **Card "Lưu ý"** (đổi tên từ "Bài học") chỉ lấy lưu ý có trong báo cáo:
   - 01: RHEL 10 cần CPU x86-64-v3, hypervisor phải chuyển tính năng CPU vào VM
   - 02: chỉ card ra Internet mới khai báo gateway, tránh hai default route
   - 03: sai một dòng fstab có thể vào emergency mode, kiểm tra `mount -a` trước khi reboot
   - 04: `-a` để thêm nhóm, không ghi đè nhóm cũ; sửa sudoers bằng visudo
   - 05: thêm `--permanent` rồi `--reload`; gán lại nhãn SELinux, không tắt SELinux
   - 07: cổng đã publish có thể truy cập từ ngoài dù chưa khai báo trong firewalld
4. **Chữ trên slide ngắn**, code tối đa 4–6 dòng mỗi khối. Phần giải thích đầy đủ đưa vào presenter notes. Slide dày dùng lưới thẻ nhỏ; cỡ chữ không nhỏ hơn 1.1rem.
5. **Presenter notes viết lại toàn bộ** theo báo cáo mới, không giữ câu nào nhắc lab hay lỗi cá nhân.
6. **Rà từ khóa** sau khi viết: tìm "Rocky", "lab xuyên suốt", "em đã", "web01 chậm", "hello-app" (trừ lệnh `set-property` có trong báo cáo), "Proxmox", "insecure", "kill -9 $(", "Bài học" trong `slides.md`. Phải còn 0 kết quả, trừ chỗ báo cáo mới có thật (ví dụ "web01" trong sơ đồ mạng phần 7, "giả lập tải" trong lộ trình GĐ1).

---

## 4. Thay đổi kỹ thuật

- **Sửa `slides.md`**: viết lại 23 slide theo mục 2. Không cần layout mới.
- **`components/LessonCard.vue`**: đổi tiêu đề mặc định "Bài học" thành "Lưu ý".
- **Sơ đồ slide 10, 17, 18**: dựng bằng HTML/CSS (như sơ đồ LVM hiện tại), không dùng Mermaid vì Mermaid bị lệch chữ khi xuất ảnh.
- **Slide Nội dung (2)**: 8 mục giữ nguyên.

---

## 5. Ảnh

Giữ 8 ảnh hiện có, không tải thêm. Ảnh dùng ở slide 1, 5, 9, 15, 23. `racks.jpg` không dùng (xem ghi chú mục 2).

---

## 6. PPTX và đưa lên web

- Xuất PPTX 23 trang sau khi anh/chị duyệt nội dung mới.
- Repo hiện chứa toàn bộ thư mục `LPC_Week_2`. Khi import vào Vercel, **Root Directory phải là `slides`**.

---

## 7. Kiểm tra trước khi báo xong

- Xuất ảnh PNG toàn bộ 23 slide, xem từng slide: tràn chữ, dấu tiếng Việt, code bị cắt, sơ đồ đúng vị trí.
- Đối chiếu nội dung từng slide với báo cáo mới (mục 1.3).
- Chạy rà từ khóa ở mục 3.6.

---

## 8. Các bước thực hiện

Mỗi bước em **hỏi và chờ anh/chị cho phép rồi mới chạy**.

1. **Viết lại nội dung** `slides.md` (23 slide) kèm presenter notes mới; đổi tên card thành "Lưu ý".
2. **Dựng 3 sơ đồ** (slide 10 storage, slide 17 đường đi gói tin, slide 18 các lớp).
3. **Kiểm tra** theo mục 7, sửa lỗi bố cục.
   - ⏸ **Điểm dừng 1:** gửi ảnh chụp toàn bộ slide (ghi đè 20 ảnh cũ trong thư mục `previews/`, thêm ảnh slide 21–23) để anh/chị duyệt.
4. **Xuất PPTX** `slides/slides-export.pptx`, mở lại để kiểm tra đủ 23 trang. Viết `slides/HUONG_DAN.md` (cách chạy, phím tắt, presenter mode).
   - ⏸ **Điểm dừng 2:** gửi file PPTX và hướng dẫn.
5. **Push lên GitHub** (commit mới, không force push).
   - ⏸ **Điểm dừng 3:** anh/chị xác nhận trước khi em push.
6. **Deploy Vercel** (anh/chị import repo, Root Directory `slides`), em kiểm tra link, thêm link và mã QR vào slide Cảm ơn, push và xuất PPTX lại.
   - ⏸ **Điểm dừng 4:** báo kết quả cuối.

---

## 9. Câu hỏi còn lại

Không còn câu hỏi về nội dung. Em chờ anh/chị cho phép chạy **bước 1 và 2** (viết lại 23 slide, dựng 3 sơ đồ), sau đó em dừng ở điểm dừng 1.

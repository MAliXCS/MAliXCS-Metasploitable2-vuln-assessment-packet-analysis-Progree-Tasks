# Review notes (not part of the repo)

Private maintainer notes for `metasploitable2-vuln-assessment-packet-analysis`.
Everything here was checked by opening each of the 23 screenshots and comparing it with the report text.

## 1. Repository identity

- **Repo name:** `metasploitable2-vuln-assessment-packet-analysis`
- **Title:** Metasploitable 2: Vulnerability Assessment & Packet Analysis Lab
- **About / short description (under 350 chars):**
  Nmap, Nessus and Nikto scans plus Wireshark traffic analysis of a deliberately vulnerable Metasploitable 2 VM. Lab write-up with screenshots from two Progree cybersecurity internship tasks.
- **Topics:** `cybersecurity` `vulnerability-assessment` `nessus` `nmap` `nikto` `wireshark` `packet-analysis` `metasploitable2` `kali-linux` `lab-report`

## 2. Evidence-to-task mapping

Status: OK = screenshot supports the caption as written. Adjusted = the report's wording did not match the screenshot, and the README follows the screenshot.

| Fig | File | Task step | What it proves | What it does NOT prove | Status |
| --- | --- | --- | --- | --- | --- |
| 1 | T2_01_nessus_prerequisites_zypper_install | 2.1 Install | `zypper` missing; `sudo apt install zypper` accepted, 12 packages | That zypper is required by Nessus (report asserts it) | OK |
| 2 | T2_02_nessus_install_dpkg | 2.1 Install | `sudo dpkg -i Nessus-10.12.4-debian10_amd64.deb`; failed first try with placeholder name | Every self-test (list is cut off) | Adjusted ("all" → "every line visible") |
| 3 | T2_03_tempmail_for_nessus_activation | 2.1 Activate | temp-mail.org open next to "Nessus / Setup" tab | An address was generated: field says "Loading." | Adjusted |
| 4 | T2_04_nessus_welcome_plugins_compiling | 2.1 Activate | Nessus Expert trial welcome, `localhost:8834`, plugins compiling | Version number | OK |
| 5 | T2_05_nessus_host_discovery_10.0.0.0-24 | 2.2 Discovery | Six hosts in 10.0.0.0/24 | That 10.0.0.16 alone was chosen (nothing ticked; still "Discovering Hosts…") | Adjusted |
| 6 | T2_06_nmap_open_ports_service_versions | 2.3 Nmap | `nmap -sV -p- 10.0.0.16`; 65,505 closed; 22 open ports listed | The other 8 of 30 open ports | OK |
| 7 | T2_07_nessus_scan_running | 2.4 Nessus | Status Running, start 3:42 AM, provisional 11/7/26/9/140, Auth Fail | Final counts | OK |
| 8 | T2_08_nessus_scan_completed_summary | 2.4 Nessus | Completed, 3:42 to 4:04, 22 min; 10 Crit, 32 Med, 8 Low, 140 Info | The High count of 3 (segment unlabeled) | OK (High marked †) |
| 9 | T2_09_nikto_web_scan_terminal | 2.5 Nikto | Nikto 2.6.1 command and first ~17 findings | 31 findings, 8,175 requests, 31 s, phpMyAdmin, `/test/` | OK (rest marked †) |
| 10 | T3_01_wireshark_welcome_screen | 3.1 Start | Wireshark 4.6.6, eth0 listed, No Packets | | OK |
| 11 | T3_02_wireshark_launch_sudo_eth0 | 3.1 Start | `sudo wireshark`, eth0 highlighted | A running capture | OK |
| 12 | T3_03_nmap_scan_seen_in_wireshark_RST_packets | 3.2 Port scan | RST/ACK burst .16 → .4:49462, 87,702 packets | That Nmap caused it (inferred); same capture as Figs 13 to 23 | OK (separate capture flagged) |
| 13 | T3_04_icmp_ping_and_dns_query | 3.3 Baseline | ICMP .4 ↔ .16; DNS query .16 → 192.168.0.1 | **No** 142.250.190.236 here | **Adjusted** |
| 14 | T3_05_icmp_and_arp_resolution | 3.3 Baseline | ICMP .16 ↔ 142.250.190.236; ARP "Who has 10.0.0.4? Tell 10.0.0.16" | The opposite ARP direction the report quotes | **Adjusted** |
| 15 | T3_06_tcp_three_way_handshake_syn_synack_ack | 3.4 Handshake | Packets 1109 to 1114 exactly as described; ARP pair at 1115 to 1118; PTR lookup | The report's ARP quote belongs here, not in Fig 14 | OK |
| 16 | T3_07_http_get_cleartext_200ok | 3.5 HTTP | GET / (1124), 200 OK text/html (1128), body in 1126 (Len 1081) | Readable page content (only the packet list is visible) | OK |
| 17 | T3_08_ftp_filter_session_overview | 3.6 FTP | 22 of 1,189 packets; 530 then 230; 500 OOPS between sessions | | OK |
| 18 | T3_09_ftp_cleartext_username | 3.6 FTP | Packet 1160 `USER msfadmin` in bytes | | OK |
| 19 | T3_10_ftp_cleartext_password | 3.6 FTP | Packet 1164 `PASS msfadmin` in bytes; 230 on next row | | OK |
| 20 | T3_11_exfil_simulation_dd_nc_refused | 3.7 Exfil | `dd … \| nc 10.0.0.16 4444` refused, **then** `dd of=/tmp/exfil.bin` | That nc tried to send the file | **Adjusted** (order) |
| 21 | T3_12_ftp_upload_exfil_10MB_complete | 3.7 Exfil | FTP `put`, EPSV 33643, 226, 78.66 MiB/s | Same login as Figs 17 to 19 (it is a separate session) | OK |
| 22 | T3_13_wireshark_exfil_stor_packets_filter | 3.7 Exfil | Filter; 266 of 2,152 (12.4%); STOR segments; Window Full | "Under a second" for all 266 (visible rows span ~20 ms) | OK |
| 23 | T3_14_follow_tcp_stream_10MB_exfil | 3.7 Exfil | Stream 7, 10 MB, 56 client / 0 server pkts | | OK |

## 3. Differences between the report and the screenshots

These are corrected in the README. Consider fixing the report too.

1. **Fig 13 vs 14.** The report says Fig 13 and 14 show ICMP to 142.250.190.236. Only Fig 14 does. Fig 13 is ICMP between the two lab hosts.
2. **ARP direction.** The report places "Who has 10.0.0.16? Tell 10.0.0.4" / "10.0.0.16 is at 08:00:27:59:5b:04" in Fig 14. That pair is in Fig 15 (packets 1115 to 1118). Fig 14 shows the reverse: 10.0.0.16 asking for 10.0.0.4.
3. **Exfil order.** The report says the file was created and then sent with netcat. The terminal shows the netcat attempt came first and piped `dd` directly; the file was created afterwards.
4. **Table 1.** "53/tcp, udp": the scan was TCP only and the screenshot shows 53/tcp.
5. **Nikto command.** The report's lab table omits the `/home/kali/` path that the screenshot shows.
6. **"Single capture".** Fig 12 (87,702 packets, live) cannot be the same capture as Fig 13 (277 packets).
7. **Third FTP login.** The report describes the failed and successful logins; the upload in Fig 21 is a further, separate login.
8. **Typo.** "EPASV" in the report is "EPSV" in Wireshark and in the terminal.

## 4. Items that need your confirmation

| # | Item | Why it matters |
| --- | --- | --- |
| 1 | **"Isolated" network.** Figs 13 and 14 show the target sending DNS to 192.168.0.1 (a `tendawifi.com` search suffix is in the query) and pinging 142.250.190.236. | The report says the lab was isolated. The captures suggest the target could reach outside 10.0.0.0/24. Confirm the VirtualBox network mode (host-only, NAT, bridged) and say so in the README. |
| 2 | **Capture date for Task 3.** Fig 21 shows `/tmp/exfil.bin` dated **Sep 25 07:39**; Nmap/Nikto say **2026-10-01**. | The report states a 1 October snapshot. Task 3 may be from 25 Sep. Figs 1 to 5 clocks (4:12 to 6:50) also sit awkwardly with Nessus scan time (3:42 AM). |
| 3 | **Fig 12 provenance.** Different packet count from Fig 13+. | Confirm it came from an earlier capture session, or re-take it from the main pcap. |
| 4 | **Student ID `B7/9`** appears inside the report .docx and in its original filename. | I renamed the file to drop it from the filename; it is still inside the document. Decide whether the .docx should be public, or export a PDF without the ID. |
| 5 | **Fig 3** shows no email address. | Fine for the README as captioned. If you want it to prove the activation step, re-take it once the address loads. |
| 6 | **Fig 8** High count (3) is not readable. | Only the report supports it. Open the Nessus scan and re-take the summary if you want it on screen. |
| 7 | **Fig 9** stops before the Nikto summary. | 31 findings / 8,175 requests / 31 s are marked †. Re-take after `Ctrl+End` or open `nikto_scan.html` if you still have it. |
| 8 | **Desktop in Figs 10, 11** shows personal-looking filenames ("My-Locked…", "note", "target ip"). | Low risk, but decide whether to crop or blur. |
| 9 | **License.** I added a `LICENSE` file that points to CC BY 4.0 and gives the deed URL. | Replace it with the full text from choosealicense.com/licenses/cc-by-4.0 if you keep CC BY 4.0, or pick another licence. |
| 10 | **Kali 2026.2, VirtualBox, Ubuntu 8.04** appear only in the report. | Marked † or kept in the badge. Remove the Kali version badge if you cannot back it up. |
| 11 | **TTL 64 on the reply from 142.250.190.236** (Fig 14). | A real internet host would not arrive with TTL 64 on a LAN. Probably the virtual gateway answering. I left TTL out of the Fig 14 caption and dropped the report's "typical of Linux" remark. |
| 12 | **Nessus start command and VM import steps** are not in any screenshot. | README says so instead of guessing. |

## 5. Final QC checklist

**Senior maintainer view: accuracy, structure, consistency**

- [x] All 23 screenshots used exactly once; none duplicated or unused (scripted check)
- [x] Each caption's figure number matches the report's Appendix A (scripted check)
- [x] Every command in the README appears in a screenshot, or is labelled as not shown
- [x] Counts re-verified on screen: 22 Nmap ports listed, 65,505 + 30 = 65,535, 193 = 10+3+32+8+140, 22 of 1,189 and 266 of 2,152 packets
- [x] Percentages recomputed: 5.2 / 1.6 / 16.6 / 4.1 / 72.5
- [x] Claims not visible in screenshots carry †
- [x] No invented features, results, commands or CVEs; Nessus finding table is copied from the report and labelled
- [x] Limitations, partial evidence and unverified claims listed in the README
- [x] No `src/` folder, because there is no source code
- [ ] You still need to resolve the confirmation items in section 4

**Documentation / visual reviewer view: readability and GitHub compatibility**

- [x] Relative image paths resolve, exact case, no spaces
- [x] Images are 1280 px or 1920 px wide, about 5.5 MB in total; fine for GitHub
- [x] `<details>`, `<table>` and code fences balanced
- [x] Three Mermaid diagrams (topology, Task 2 workflow, exfil sequence) use basic syntax that GitHub renders
- [x] Setup screenshots collapsed; result screenshots shown inline
- [x] Alt text on every image
- [x] One badge row, six static badges, no live-service badges that could break
- [ ] Mermaid not rendered here (no network). Open the README on GitHub once and check all three diagrams
- [ ] Check dark mode once: screenshots are dark, so they should look fine

## 6. GIF / animation recommendations

Record these from a fresh lab run. I did not make GIFs from the existing stills, because that would imply a live recording that did not happen. Keep each under about 5 MB, store in `docs/media/`, and add them under the matching step.

1. **Clear-text FTP login (Task 3.6), about 10 s.** Apply `ip.addr == 10.0.0.16 && ftp`, click the `PASS` packet, show the bytes pane. Replaces the need to read Figs 18 and 19 separately.
2. **Exfil end to end (Task 3.7), about 20 s.** `ftp` upload in a terminal, then Wireshark with `tcp.len > 500`, then Follow TCP Stream. Shows the cause and effect that Figs 21 to 23 only show as stills.
3. **Port scan footprint (Task 3.2), about 10 s.** Nmap running in one pane, Wireshark filling with RST/ACK in the other. This would also settle the Fig 12 provenance question.

Tools: `peek` (GIF), or `asciinema` + `agg` for terminal-only clips.
Not worth animating: Nessus install and the 22-minute scan.

## 7. Suggested next steps on GitHub

1. `git init`, add files, push.
2. Set the About text and topics from section 1.
3. Open the README once on github.com and check the three diagrams.
4. Resolve section 4 items 1 to 4 before making the repo public.

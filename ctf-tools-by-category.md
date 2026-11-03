# CTF Tools by Challenge Category (200+ Free / Open-Source)

Organized by the standard CTF problem formats. Every tool is free and open source;
most run fully offline, a few are API-based (marked **[API]**) and a few are AI-agent
tools needing an LLM backend — local (Ollama) or free-tier API (Gemini/Groq/OpenRouter),
marked **[AI]**.

Each tool has: what it's **for**, and a quick **usage** line where relevant.

---

## 0. Encoding & Classical Cipher Identification

*Use this category first whenever challenge text "looks wrong" — doesn't look like normal
language, has odd character sets, repeating patterns, or is clearly not plaintext. This is
different from "Cryptography" (section 5) below: this section is about **identifying and
reversing an encoding/classical cipher**, not breaking modern crypto math.*

1. [CyberChef](https://github.com/gchq/CyberChef) — the single most important tool for this category. Chains decode/encode/hash/cipher operations visually.
   **Use for:** Base64/32/85, hex, binary, URL-encoding, ROT13/ROT47, Vigenère, Atbash, Morse, XOR, gzip/zlib, and 300+ other transforms.
   **Usage:** Open `CyberChef.html` (or [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/) if offline hosting isn't needed) → drag the "Magic" operation onto the input to auto-detect likely encodings, or manually stack operations (e.g. "From Base64" → "From Hex" → "XOR Brute Force").
2. [dcode.fr-style offline cipher identifier scripts](https://github.com/Zabanbat/cipher-identifier) — **for:** guessing which classical cipher (Caesar, Vigenère, Playfair, rail fence, etc.) produced a ciphertext, based on letter-frequency analysis.
3. [quipqiup](http://quipqiup.com/) **[web, free, no signup]** — **for:** automatic solving of substitution ciphers (monoalphabetic) using a dictionary-based solver. Paste ciphertext, it brute-forces the substitution.
4. [Caesar/ROT brute-forcer (CyberChef "ROT13 Brute Force")] — built into CyberChef above; **for:** quickly trying all 25 shifts.
5. [base64 / base32 / base85 CLI (coreutils `base64`, `base32`)] — **for:** quick one-off decode without opening a GUI. **Usage:** `echo "<string>" | base64 -d`
6. [xxd / hexdump](https://man7.org/linux/man-pages/man1/xxd.1.html) — **for:** viewing raw hex/ASCII of a file or string. **Usage:** `xxd file.bin | less`
7. [Morse code decoders (CyberChef "From Morse Code")] — **for:** `.-.-` style text or audio beeps transcribed to dots/dashes.
8. [Brainfuck / esolang interpreters](https://github.com/search?q=esolang+interpreter) — **for:** challenge text that's actually an esoteric programming language payload, not a cipher at all.
9. [z3-solver](https://github.com/Z3Prover/z3) — **for:** when the "encoding" is actually a constraint system (e.g., a custom cipher described as equations). **Usage:** model the cipher's math in Python with `z3`, let the solver invert it.
10. [RsaCtfTool](https://github.com/RsaCtfTool/RsaCtfTool) — **for:** when the "weird format" turns out to be RSA-encoded data (see section 5 too).
11. [file / libmagic](https://github.com/file/file) — **for:** always run this *first* on any blob — tells you if it's actually a known file format wearing a disguise, not text at all. **Usage:** `file challenge.bin`
12. [binvis.io-style visualizers / binwalk](https://github.com/ReFirmLabs/binwalk) — **for:** spotting embedded structure (magic bytes, compressed streams) inside a "text" blob.

**Typical workflow for "weird text":**
`file` the blob → eyeball charset (hex? base64 alphabet? only A-Z? only dots/dashes?) → CyberChef Magic wand → if it's a cipher not an encoding, try quipqiup / frequency analysis → if numeric/structured, consider it might be crypto (section 5) not just encoding.

---

## 1. Web Exploitation

*Use when the challenge gives you a URL / web app.*

1. [Burp Suite Community](https://portswigger.net/burp/communitydownload) — **for:** intercepting/modifying HTTP requests. **Usage:** set browser proxy to 127.0.0.1:8080, browse the target, send interesting requests to Repeater.
2. [OWASP ZAP](https://github.com/zaproxy/zaproxy) — free Burp alternative with built-in automated scanning. **Usage:** `zap.sh -quickurl http://target`
3. [sqlmap](https://github.com/sqlmapproject/sqlmap) — **for:** SQL injection. **Usage:** `sqlmap -u "http://target/?id=1" --dbs`
4. [ffuf](https://github.com/ffuf/ffuf) — **for:** fast directory/parameter fuzzing. **Usage:** `ffuf -w wordlist.txt -u http://target/FUZZ`
5. [gobuster](https://github.com/OJ/gobuster) — directory/DNS/vhost brute-forcer. **Usage:** `gobuster dir -u http://target -w wordlist.txt`
6. [feroxbuster](https://github.com/epi052/feroxbuster) — recursive content discovery, good for nested hidden paths.
7. [wfuzz](https://github.com/xmendez/wfuzz) — general-purpose web fuzzer (params, headers, cookies).
8. [nikto](https://github.com/sullo/nikto) — automated web server misconfig/CVE scanner.
9. [Nuclei](https://github.com/projectdiscovery/nuclei) — template-based vuln scanner. **Usage:** `nuclei -u http://target -t cves/`
10. [httpx](https://github.com/projectdiscovery/httpx) — probes live hosts, tech stack, status codes in bulk.
11. [XSStrike](https://github.com/s0md3v/XSStrike) — **for:** XSS detection with context-aware payload generation.
12. [Commix](https://github.com/commixproject/commix) — **for:** OS command injection discovery/exploitation.
13. [tplmap](https://github.com/epinna/tplmap) — **for:** server-side template injection (Jinja2, Twig, etc.).
14. [NoSQLMap](https://github.com/codingo/NoSQLMap) — MongoDB/NoSQL injection.
15. [JWT_Tool](https://github.com/ticarpi/jwt_tool) — **for:** JWT tampering (alg=none, key confusion, signature cracking). **Usage:** `python3 jwt_tool.py <token> -T`
16. [Arjun](https://github.com/s0md3v/Arjun) — discovers hidden HTTP parameters.
17. [ParamSpider](https://github.com/devanshbatham/ParamSpider) — mines parameters from web archives (recon).
18. [GraphQLmap](https://github.com/swisskyrepo/GraphQLmap) — GraphQL endpoint exploitation/introspection.
19. [Postman](https://github.com/postmanlabs) / [Insomnia](https://github.com/Kong/insomnia) — manual API request crafting GUIs.
20. [CyberChef](https://github.com/gchq/CyberChef) — decoding cookies, tokens, odd response bodies (cross-listed from section 0).
21. [Corsy](https://github.com/s0md3v/Corsy) — CORS misconfiguration scanner.
22. [SSRFmap](https://github.com/swisskyrepo/SSRFmap) — SSRF exploitation framework.
23. [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) — **for:** reference, not a runnable tool — the payload cheat-sheet you grep when stuck.
24. [HackTricks](https://github.com/HackTricks-wiki/hacktricks) — methodology wiki, use to look up "how do I escalate from X" for almost any category.

## 2. Reverse Engineering

*Use when given a binary/executable and asked to find hidden logic, keys, or a flag check.*

25. [Ghidra](https://github.com/NationalSecurityAgency/ghidra) — **for:** static disassembly + decompilation. **Usage:** open binary, let auto-analysis run, read decompiled C in the Decompile window, search for string refs to find flag-check functions.
26. [radare2](https://github.com/radareorg/radare2) — CLI RE framework. **Usage:** `r2 -A binary` then `afl` (list functions), `pdf @main` (disasm main).
27. [Cutter](https://github.com/rizinorg/cutter) — GUI frontend for radare2/rizin, easier for beginners than raw r2.
28. [rizin](https://github.com/rizinorg/rizin) — actively maintained radare2 fork.
29. [x64dbg](https://github.com/x64dbg/x64dbg) — **for:** Windows dynamic debugging (step through, set breakpoints on compare instructions).
30. [dnSpyEx](https://github.com/dnSpyEx/dnSpy) — **for:** .NET binaries — decompile and even patch/debug C# directly.
31. [JADX](https://github.com/skylot/jadx) — **for:** Android APK → readable Java source.
32. [Apktool](https://github.com/iBotPeaches/Apktool) — **for:** unpacking/repacking APKs to smali for patching.
33. [Frida](https://github.com/frida/frida) — **for:** dynamic instrumentation — hook functions at runtime to dump keys/bypass checks without patching the binary. **Usage:** write a small JS hook script, `frida -U -f com.app -l hook.js`
34. [objection](https://github.com/sensepost/objection) — Frida wrapper, no-setup runtime mobile exploration.
35. [angr](https://github.com/angr/angr) — **for:** symbolic execution — automatically *find the input* that reaches a "flag correct" branch instead of reverse-engineering the logic by hand.
36. [Capstone](https://github.com/capstone-engine/capstone) — disassembly library, used when scripting custom analysis.
37. [Keystone](https://github.com/keystone-engine/keystone) — assembler library, for scripting binary patches.
38. [Unicorn Engine](https://github.com/unicorn-engine/unicorn) — CPU emulator for running isolated code snippets.
39. [Qiling](https://github.com/qilingframework/qiling) — full binary emulation (great for firmware/embedded rev).
40. [Detect It Easy](https://github.com/horsicq/Detect-It-Easy) — **for:** identifying packers/compilers before you waste time reversing a packed binary.
41. [PE-bear](https://github.com/hasherezade/pe-bear) — PE header/section inspection for Windows binaries.
42. [UPX](https://github.com/upx/upx) — **Usage:** `upx -d packed.exe` to unpack UPX-packed binaries.
43. [retdec](https://github.com/avast/retdec) — automated machine-code-to-C decompiler.
44. [ILSpy](https://github.com/icsharpcode/ILSpy) — .NET decompiler, alternative to dnSpy.
45. [GhidraMCP](https://github.com/LaurieWired/GhidraMCP) **[AI]** — lets an LLM drive Ghidra's decompiler/renamer for you via chat.
46. [IDA Pro MCP](https://github.com/mrexodia/ida-pro-mcp) **[AI]** — same idea for IDA Pro.
47. [JADX MCP Server](https://github.com/zinja-coder/jadx-mcp-server) **[AI]** — LLM-assisted Android decompilation.
48. [r2ai](https://github.com/radareorg/r2ai) **[AI]** — `r2ai -d` to have a local/remote LLM decompile+explain the current function inside radare2.
49. [r2d2](https://github.com/sandbornm/r2d2) **[AI]** — Claude-powered copilot orchestrating radare2+Ghidra+angr+GDB together.

## 3. Binary Exploitation / Pwn

*Use when given a vulnerable binary + remote `nc host port` service, and the goal is to pop a shell or leak memory.*

50. [pwntools](https://github.com/Gallopsled/pwntools) — **for:** the standard Python exploit-writing library. **Usage:** `from pwn import *; p = remote('host', port); p.sendline(payload)`
51. [pwndbg](https://github.com/pwndbg/pwndbg) — **for:** GDB plugin that visualizes stack/heap/registers while exploiting. **Usage:** `gdb ./binary` then it auto-loads if installed.
52. [GEF](https://github.com/hugsy/gef) — alternative GDB enhancement, similar purpose to pwndbg.
53. [pwninit](https://github.com/io12/pwninit) — **for:** auto-detects binary/libc/loader in a challenge folder and sets up a template exploit script.
54. [ROPgadget](https://github.com/JonathanSalwan/ROPgadget) — **for:** finding ROP gadgets when direct shellcode injection is blocked (NX). **Usage:** `ROPgadget --binary ./chall`
55. [one_gadget](https://github.com/david942j/one_gadget) — **for:** finds single libc addresses that directly spawn a shell. **Usage:** `one_gadget libc.so.6`
56. [libc-database](https://github.com/niklasb/libc-database) — **for:** identifying the exact libc version from leaked addresses, to get correct offsets.
57. [patchelf](https://github.com/NixOS/patchelf) — **for:** forcing a binary to use a specific libc/loader locally to match the remote environment.
58. [checksec.sh](https://github.com/slimm609/checksec.sh) — **for:** first command to run on any pwn binary — shows which mitigations (NX, PIE, Canary, RELRO) are on, which decides your exploit strategy.
59. [pwncat](https://github.com/calebstewart/pwncat) — post-exploitation shell handling after you get a foothold.
60. [ropper](https://github.com/sashs/Ropper) — alternate gadget-finder, sometimes finds gadgets ROPgadget misses.
61. [angr](https://github.com/angr/angr) — symbolic exploit-path discovery (cross-listed).
62. [AFL++](https://github.com/AFLplusplus/AFLplusplus) — **for:** fuzzing a binary to find the crashing input in the first place.
63. [Triton](https://github.com/JonathanSalwan/Triton) — dynamic symbolic execution, used for constraint-solving exploits.
64. [gef-extras](https://github.com/hugsy/gef-extras) — community add-ons (heap visualizers etc.) for GEF.

## 4. Cryptography (Modern/Math-based)

*Use when the challenge is explicitly "crypto" — RSA, AES, ECC, hash length-extension, etc. — distinct from section 0's text-encoding puzzles.*

65. [CyberChef](https://github.com/gchq/CyberChef) — quick symmetric cipher ops, hashing, HMAC checks.
66. [RsaCtfTool](https://github.com/RsaCtfTool/RsaCtfTool) — **for:** automated RSA attacks. **Usage:** `python3 RsaCtfTool.py --publickey key.pub --uncipherfile flag.enc`
67. [SageMath](https://github.com/sagemath/sage) — **for:** lattice attacks, elliptic curve math, anything needing real number theory.
68. [FeatherDuster](https://github.com/nccgroup/featherduster) — **for:** automatically spotting weaknesses in a custom crypto implementation.
69. [Hashcat](https://github.com/hashcat/hashcat) — **for:** GPU-accelerated hash cracking. **Usage:** `hashcat -m 0 hash.txt wordlist.txt`
70. [John the Ripper](https://github.com/openwall/john) — CPU password/hash cracker, good for unusual hash formats hashcat doesn't support.
71. [SecLists](https://github.com/danielmiessler/SecLists) — wordlists for the crackers above.
72. [xortool](https://github.com/hellman/xortool) — **for:** guessing unknown XOR key length and the key itself from repeated-key XOR ciphertext.
73. [PyCryptodome](https://github.com/Legrandin/pycryptodome) — **for:** scripting your own attack when no existing tool fits (padding oracle, custom AES mode misuse, etc.).
74. [padding-oracle-attacker](https://github.com/KishanBagaria/padding-oracle-attacker) — automates CBC padding oracle attacks against a live endpoint.
75. [fpylll](https://github.com/fplll/fpylll) — lattice reduction (LLL) for breaking weak RSA/ECC/knapsack schemes.
76. [z3-solver](https://github.com/Z3Prover/z3) — constraint solving for custom/broken crypto schemes.
77. [yafu](https://github.com/bbuhrow/yafu) — **for:** factoring large semiprimes (weak RSA moduli).

## 5. Forensics

*Use for memory dumps, disk images, or "something happened on this system, find the flag."*

78. [Volatility 3](https://github.com/volatilityfoundation/volatility3) — **for:** memory dump analysis. **Usage:** `vol3 -f mem.dmp windows.pslist` to list processes, then dig into suspicious ones.
79. [Autopsy](https://github.com/sleuthkit/autopsy) — GUI for full disk-image forensic investigation.
80. [The Sleuth Kit](https://github.com/sleuthkit/sleuthkit) — CLI file-system forensic toolkit (powers Autopsy).
81. [binwalk](https://github.com/ReFirmLabs/binwalk) — **for:** extracting embedded files from any blob. **Usage:** `binwalk -e file.bin`
82. [foremost](https://github.com/korczis/foremost) — file carving from raw disk images by file signature.
83. [scalpel](https://github.com/sleuthkit/scalpel) — alternative file carver, faster on large images.
84. [Wireshark](https://github.com/wireshark/wireshark) — **for:** PCAP analysis. **Usage:** open the .pcap, use "Follow TCP Stream" on suspicious conversations.
85. [NetworkMiner](https://www.netresec.com/?page=NetworkMiner) — auto-extracts files/credentials from PCAPs (free edition).
86. [tshark](https://github.com/wireshark/wireshark) — CLI version of Wireshark for scripted PCAP analysis.
87. [CapTipper](https://github.com/omriher/CapTipper) — analyzes malicious HTTP traffic captured in a PCAP.
88. [Plaso/log2timeline](https://github.com/log2timeline/plaso) — builds a forensic timeline from many artifact types.
89. [ExifTool](https://github.com/exiftool/exiftool) — **Usage:** `exiftool image.jpg` — metadata often hides the flag directly.
90. [bulk_extractor](https://github.com/simsong/bulk_extractor) — scans raw disk images for emails, URLs, credit cards, etc.
91. [Rekall](https://github.com/google/rekall) — alternative memory forensics framework.
92. [dumpzilla](https://github.com/Busindre/dumpzilla) — Firefox/browser forensic artifact extraction.
93. [RegRipper](https://github.com/keydet89/RegRipper3.0) — Windows registry hive parsing.
94. [Eric Zimmerman's tools](https://github.com/EricZimmerman) — suite for parsing Windows artifacts (MFT, prefetch, shellbags).
95. [TestDisk/PhotoRec](https://github.com/cgsecurity/testdisk) — recovers deleted files/partitions.

## 6. Steganography

*Use for "here's an image/audio/file, the flag is hidden inside it."*

96. [zsteg](https://github.com/zed-0xff/zsteg) — **Usage:** `zsteg image.png` — auto-checks common LSB-steg patterns in PNG/BMP.
97. [steghide](https://github.com/StefanoDeVuono/steghide) — **Usage:** `steghide extract -sf image.jpg` (prompts for passphrase).
98. [stegseek](https://github.com/RickdeJager/stegseek) — **for:** fast dictionary-attack against a steghide passphrase. **Usage:** `stegseek image.jpg wordlist.txt`
99. [StegCracker](https://github.com/Paradoxis/StegCracker) — alternative steghide brute-forcer.
100. [Stegsolve](https://github.com/zardus/ctf-tools/blob/master/stegsolve/) — **for:** manually flipping through bit-planes/color-channel filters to spot hidden visual data.
101. [outguess](https://github.com/crorvick/outguess) — steganography tool for JPEG.
102. [binwalk](https://github.com/ReFirmLabs/binwalk) — also catches files-within-files hidden steg-style (cross-listed).
103. [exiftool](https://github.com/exiftool/exiftool) — flags are often just stuffed in a metadata field (cross-listed).
104. [pngcheck](http://www.libpng.org/pub/png/apps/pngcheck.html) — **for:** validating PNG chunk structure, spotting extra/corrupted chunks hiding data.
105. [stego-toolkit](https://github.com/DominicBreuker/stego-toolkit) — Docker image bundling most of the above so you don't install each separately.
106. [Sonic Visualiser](https://github.com/sonic-visualiser/sonic-visualiser) — **for:** audio challenges — view the spectrogram, text/QR codes are sometimes drawn into the frequency domain.
107. [Audacity](https://github.com/audacity/audacity) — audio editing, also has a spectrogram view mode.

## 7. OSINT

*Use for "find information about this person/company/image using public sources."*

108. [theHarvester](https://github.com/laramies/theHarvester) — email/subdomain/name harvesting from search engines.
109. [SpiderFoot](https://github.com/smicallef/spiderfoot) — automated, broad OSINT reconnaissance across 200+ modules.
110. [Recon-ng](https://github.com/lanmaster53/recon-ng) — modular OSINT framework, Metasploit-style workflow.
111. [Sherlock](https://github.com/sherlock-project/sherlock) — **Usage:** `python3 sherlock.py username` — finds accounts across hundreds of sites.
112. [Maltego CE](https://www.maltego.com/community-edition/) — visual link-analysis for connecting OSINT entities.
113. [ExifTool](https://github.com/exiftool/exiftool) — GPS/geolocation metadata in photos (cross-listed).
114. [GHunt](https://github.com/mxrch/GHunt) — Google account OSINT (Gaia ID, calendar, photos exposure).
115. [Holehe](https://github.com/megadose/holehe) — checks which services an email is registered on.
116. [PhoneInfoga](https://github.com/sundowndev/phoneinfoga) — phone number OSINT.
117. [OSINT Framework](https://github.com/lockfale/osint-framework) — curated directory of OSINT resources by category (reference, not a tool).
118. [Amass](https://github.com/owasp-amass/amass) — subdomain/attack-surface mapping.
119. [Overpass Turbo](https://github.com/osmlab/overpass-turbo) — **for:** "where was this photo taken" geolocation-style challenges using OpenStreetMap queries.

## 8. Network / Protocol

*Use for live network services, packet crafting, or pivoting challenges.*

120. [Wireshark](https://github.com/wireshark/wireshark) (cross-listed)
121. [Scapy](https://github.com/secdev/scapy) — **for:** scripting custom packet crafting/parsing in Python.
122. [tcpdump](https://www.tcpdump.org/) — CLI packet capture. **Usage:** `tcpdump -i eth0 -w out.pcap`
123. [nmap](https://github.com/nmap/nmap) — **Usage:** `nmap -sV -p- target` — port/service discovery, usually step one on any network challenge.
124. [masscan](https://github.com/robertdavidgraham/masscan) — very fast full-range port scanning.
125. [ncat (nmap suite)](https://github.com/nmap/nmap) — netcat-style TCP/UDP swiss-army knife for interacting with a service.
126. [chisel](https://github.com/jpillora/chisel) — **for:** TCP tunneling to pivot through a compromised host to an internal network.
127. [ligolo-ng](https://github.com/nicocha30/ligolo-ng) — alternative pivoting tunnel tool.
128. [dnschef](https://github.com/iphelix/dnschef) — fake DNS server for protocol-manipulation challenges.
129. [Responder](https://github.com/lgandx/Responder) — LLMNR/NBT-NS poisoning, common in Active-Directory-style network CTFs.

## 9. Mobile (Android / iOS)

130. [JADX](https://github.com/skylot/jadx) (cross-listed)
131. [Apktool](https://github.com/iBotPeaches/Apktool) (cross-listed)
132. [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) — **for:** automated static+dynamic APK/IPA security scan pipeline, start here before manual rev.
133. [Frida](https://github.com/frida/frida) (cross-listed)
134. [objection](https://github.com/sensepost/objection) (cross-listed)
135. [drozer](https://github.com/WithSecureLabs/drozer) — **for:** testing exposed Android components (activities, content providers).
136. [apkleaks](https://github.com/dwisiswant0/apkleaks) — scans an APK for hardcoded secrets/API endpoints.
137. [class-dump](https://github.com/nygard/class-dump) — dumps Objective-C class headers from an iOS binary.
138. [checkra1n](https://checkra.in/) — iOS jailbreak for research/analysis use on older devices.

## 10. Hardware / IoT / Embedded

139. [binwalk](https://github.com/ReFirmLabs/binwalk) (cross-listed) — first tool for any firmware dump.
140. [firmware-mod-kit](https://github.com/rampageX/firmware-mod-kit) — unpack/repack firmware images.
141. [EMBA](https://github.com/e-m-b-a/emba) — **for:** automated firmware security analysis pipeline.
142. [FACT_core](https://github.com/fkie-cad/FACT_core) — firmware comparison/analysis platform.
143. [flashrom](https://github.com/flashrom/flashrom) — read/write SPI flash chips (physical hardware access).
144. [JTAGulator](https://github.com/grandideastudio/jtagulator) — identifies JTAG/UART pinouts on unknown hardware.
145. [PulseView (sigrok)](https://github.com/sigrokproject/pulseview) — logic analyzer GUI, for reading signal traces off a board.
146. [QEMU](https://github.com/qemu/qemu) — **for:** emulating firmware/architectures you don't have physical hardware for.
147. [avrdude](https://github.com/avrdudes/avrdude) — read/write AVR microcontroller firmware.

## 11. Cloud Security (AWS/Azure/GCP/K8s)

148. [Pacu](https://github.com/RhinoSecurityLabs/pacu) — **for:** AWS post-compromise exploitation framework.
149. [ScoutSuite](https://github.com/nccgroup/ScoutSuite) — multi-cloud misconfiguration auditing.
150. [Prowler](https://github.com/prowler-cloud/prowler) — AWS/Azure/GCP security assessment against CIS benchmarks.
151. [CloudGoat](https://github.com/RhinoSecurityLabs/cloudgoat) — **for:** practicing on a deliberately-vulnerable AWS environment.
152. [kube-hunter](https://github.com/aquasecurity/kube-hunter) — Kubernetes penetration testing.
153. [kube-bench](https://github.com/aquasecurity/kube-bench) — CIS Kubernetes benchmark compliance checks.
154. [Trivy](https://github.com/aquasecurity/trivy) — container image/IaC vulnerability scanning.
155. [Peirates](https://github.com/inguardians/peirates) — Kubernetes penetration tool, good for privesc-within-cluster challenges.
156. [S3Scanner](https://github.com/sa7mon/S3Scanner) — finds and checks permissions on open S3 buckets.

## 12. Blockchain / Smart Contract / Web3

157. [Foundry](https://github.com/foundry-rs/foundry) — **for:** the standard Solidity dev/exploit toolkit (forge/cast/anvil). **Usage:** `forge test` against the local anvil fork to prototype an exploit transaction.
158. [Slither](https://github.com/crytic/slither) — static analyzer, flags common Solidity vuln patterns fast.
159. [Mythril](https://github.com/Consensys/mythril) — symbolic execution over EVM bytecode.
160. [Echidna](https://github.com/crytic/echidna) — property-based smart contract fuzzer.
161. [Manticore](https://github.com/trailofbits/manticore) — symbolic execution for EVM and native binaries.
162. [ethers.js](https://github.com/ethers-io/ethers.js) / [web3.py](https://github.com/ethereum/web3.py) — scripting the actual exploit transaction once you know the vuln.
163. [Damn Vulnerable DeFi](https://github.com/theredguild/damn-vulnerable-defi) — practice set, not a tool — solve these to learn the patterns.
164. [Ethernaut](https://github.com/OpenZeppelin/ethernaut) — OpenZeppelin's beginner-to-advanced solidity wargame.

## 13. Misc / Scripting / Sandbox Escapes

165. [CyberChef](https://github.com/gchq/CyberChef) (cross-listed) — covers most "weird encoding" misc challenges.
166. [cipher-identifier](https://github.com/Zabanbat/cipher-identifier) (cross-listed from section 0)
167. [z3-solver](https://github.com/Z3Prover/z3) — program-logic/puzzle misc challenges.
168. [PyJail-breakout](https://github.com/chrispetrou/PyJail-breakout) — **for:** reference techniques to escape restricted Python sandboxes (`__builtins__` tricks etc.).
169. [GTFOBins](https://github.com/GTFOBins/GTFOBins.github.io) — **for:** reference — once you have limited shell access to a Unix binary, look it up here for privesc/breakout one-liners.
170. [LOLBAS](https://github.com/LOLBAS-Project/LOLBAS) — the Windows equivalent of GTFOBins.
171. [file / libmagic](https://github.com/file/file) — always the first command on any unidentified misc blob.

## 14. AI / LLM Security CTF (prompt injection, jailbreaks, agent abuse)

*A newer but increasingly common CTF category — the "binary" is an LLM or an agent, and you exploit it with crafted text instead of crafted bytes.*

172. [garak](https://github.com/NVIDIA/garak) — **Usage:** `garak --model_type ollama --model_name llama3 --probes promptinject` — automated probing for known LLM weaknesses.
173. [PyRIT](https://github.com/microsoft/PyRIT) — Microsoft's structured red-teaming framework for generative AI.
174. [promptfoo](https://github.com/promptfoo/promptfoo) — 100+ attack types, good for systematically testing a target chatbot.
175. [LLM Guard](https://github.com/protectai/llm-guard) — **for:** understanding what a defense looks like, to figure out how to bypass one in a challenge.
176. [Rebuff](https://github.com/protectai/rebuff) — prompt-injection detector (useful to study detection logic you're trying to evade).
177. [Vigil-LLM](https://github.com/deadbits/vigil-llm) — another injection/jailbreak detector, same use case.
178. [GPTFuzz](https://github.com/sherdencooper/GPTFuzz) — fuzzing-based jailbreak prompt generation.
179. [HouYi](https://github.com/LLMSecurity/HouYi) — prompt injection specifically against LLM-integrated apps (not the raw model).
180. [EasyJailbreak](https://github.com/EasyJailbreak/EasyJailbreak) — generates adversarial jailbreak prompts systematically.
181. [DeepTeam](https://github.com/confident-ai/deepteam) — OWASP-LLM-Top-10-aligned automated red teaming.
182. [Agentic Security](https://github.com/msoedov/agentic_security) — jailbreak/fuzz scanner aimed at agent pipelines, not just chat models.
183. [PurpleLlama](https://github.com/meta-llama/PurpleLlama) — Meta's CyberSecEval benchmark suite + Llama Guard/Prompt Guard.
184. [FuzzyAI](https://github.com/cyberark/FuzzyAI) — automated jailbreak fuzzer.
185. [Open-Prompt-Injection](https://github.com/liu00222/Open-Prompt-Injection) — toolkit/benchmark, good reference implementation of attack techniques.
186. [AI-Goat](https://github.com/dhammon/ai-goat) — **for:** practicing offline, local-LLM CTF challenges mapped to OWASP LLM Top 10.
187. [Damn Vulnerable LLM Agent](https://github.com/ReversecLabs/damn-vulnerable-llm-agent) — practice target: force a ReAct agent into SQLi via prompt injection.
188. [Gandalf (Lakera)](https://gandalf.lakera.ai) **[API, browser]** — free, no-signup prompt-injection ladder, good for building intuition fast.
189. [Arcanum PI Taxonomy](https://github.com/Arcanum-Sec/arc_pi_taxonomy) — reference classification of injection/jailbreak attack types.

## Bonus: Autonomous AI Agents That Solve CTFs End-to-End **[AI]**

190. [BoxPwnr](https://github.com/0ca/BoxPwnr) — benchmarks LLM agents on HTB/picoCTF/PortSwigger/TryHackMe.
191. [Project Muteki](https://github.com/FishCodeTech/muteki) — multi-agent CTF-solving swarm across all categories.
192. [ctf-agent](https://github.com/verialabs/ctf-agent) — races multiple AI models per challenge in Docker sandboxes.
193. [ctf-kit](https://github.com/clone4truth/ctf-kit) — 210 CTF tools exposed via MCP + autonomous solver loop.
194. [PentestGPT](https://github.com/GreyDGL/PentestGPT) — LLM-guided pentest/CTF reasoning assistant.
195. [NYU CTF Bench](https://github.com/NYU-LLM-CTF/NYU_CTF_Bench) + [llm_ctf_automation](https://github.com/NYU-LLM-CTF/llm_ctf_automation) — the standard open academic benchmark/harness.
196. [Cybench](https://github.com/andyzorigin/cybench) — 40 professional-level CTF tasks + agent evaluation framework.
197. [hackingBuddyGPT](https://github.com/ipa-lab/hackingBuddyGPT) — academic LLM ethical-hacking agent framework, ~50 LOC to extend.

## Reference / Meta Lists (not tools themselves, but point to more)

198. [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) (cross-listed)
199. [HackTricks](https://github.com/HackTricks-wiki/hacktricks) (cross-listed)
200. [awesome-ctf](https://github.com/apsdehal/awesome-ctf) — curated meta-list of CTF frameworks, libraries, and resources across every category above.

---

### General workflow for any CTF challenge
1. Read the prompt + category tag carefully — it usually tells you which section above to start in.
2. `file` anything you're given, always, first.
3. Check challenge points/difficulty — low points usually means "don't overthink it," the answer is often a single obvious tool run.
4. For binaries: `checksec` → `file` → open in Ghidra/r2 before touching a debugger.
5. For text: `file` → CyberChef Magic wand → only move to real crypto tools if it's clearly modern math, not an encoding.
6. Keep a scratch terminal with pwntools/CyberChef/z3 open — most CTF solving is "glue code between two tools," not using any single tool perfectly.
7. If genuinely stuck after a real attempt, search the exact challenge/service name — many CTF challenges reuse known CVEs or known challenge templates.

### Notes on using these for free
- **Fully offline**: everything except `[AI]` and `[API]`-tagged tools needs no internet or account.
- **For `[AI]` tools**: run the LLM backend via **Ollama** for zero cost and full offline use, or a free-tier API (Gemini, Groq, OpenRouter) for a stronger model without local GPU.
- **Competition rules**: many live CTFs explicitly ban AI-agent/autonomous solving — check the ruleset before using anything tagged `[AI]`.

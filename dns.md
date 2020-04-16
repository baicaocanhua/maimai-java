netsh interface ip delete dnsservers "本地连接" 152.22.247.132



netsh interface ip add dns name="本地连接" addr=175.22.247.12 index=2

ipconfig /flushdns
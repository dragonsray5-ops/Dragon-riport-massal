# Dragon-riport-massal
Ddos
import sys
import threading
import requests
from PyQt5.QtWidgets import QApplication, QWidget, QLabel, QPushButton, QLineEdit, QVBoxLayout
import time

# == HEADER ASCII TERMINAL: DRAGONS STANDBY RAY ==
def ascii_header():
    print("\033[1;36m")
    print("╔══════════════════════════════════════════════════════════════════════════════════════════════════════╗")
    print("║ DRAGONS SYSTEM STANDBY MODE INITIATED                                                             ║")
    print("║ ▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒║")
    print("║ STATUS    → OPTICAL LINK STABILIZED                                                                ║")
    print("║ CHANNEL   → CYBERSTREAM ACTIVE                                                                     ║")
    print("║ MISSION   → SIGNALING REPORT ROUTE TO STRIKE CORE                                                  ║")
    print("║ LEVEL     → THETA FRACTAL :: PHASE PULSE                                                           ║")
    print("║ STREAMING MODE ENGAGED — REPORT WAVE INITIALIZED                                                   ║")
    print("╚══════════════════════════════════════════════════════════════════════════════════════════════════════╝\033[0m")

# == Function: Kirim laporan via requests ==
def start_reporting(endpoint, target_id, reason, token, total):
    ascii_header()
    MAX = 20000
    if total > MAX:
        print(f"\033[1;33m[!] Jumlah laporan melebihi batas ({MAX}) — disetel ke {MAX}.\033[0m")
        total = MAX

    headers = {
        "Authorization": f"Bearer {token}",
        "Content-Type": "application/json"
    }

    payload = {
        "target_id": target_id,
        "reason": reason
    }

    for i in range(1, total + 1):
        try:
            response = requests.post(endpoint, json=payload, headers=headers)
            if response.status_code == 200 and ("success" in response.text or "true" in response.text):
                print(f"\033[1;31m[# {i:05}] attack IP → status: successful report sent to servers\033[0m")
            else:
                print(f"\033[1;33m[# {i:05}] Unrecognized response: {response.status_code} — {response.text}\033[0m")
        except Exception as e:
            print(f"\033[1;31m[# {i:05}] Failed: {e}\033[0m")
        time.sleep(0.2)

# == GUI PyQt5 ==
class ReportApp(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("DRAGONS STANDBY STRIKE PANEL")
        self.setFixedSize(420, 360)
        layout = QVBoxLayout()

        self.endpoint = QLineEdit()
        self.endpoint.setPlaceholderText("Masukkan endpoint TikTok")
        layout.addWidget(QLabel("Endpoint"))
        layout.addWidget(self.endpoint)

        self.target = QLineEdit()
        self.target.setPlaceholderText("ID pengguna target")
        layout.addWidget(QLabel("Target ID"))
        layout.addWidget(self.target)

        self.reason = QLineEdit()
        self.reason.setPlaceholderText("Alasan pelaporan")
        layout.addWidget(QLabel("Alasan"))
        layout.addWidget(self.reason)

        self.token = QLineEdit()
        self.token.setPlaceholderText("Authorization Token")
        layout.addWidget(QLabel("Token"))
        layout.addWidget(self.token)

        self.total = QLineEdit()
        self.total.setPlaceholderText("Jumlah laporan (maksimal 20000)")
        layout.addWidget(QLabel("Total"))
        layout.addWidget(self.total)

        self.start_btn = QPushButton("MULAI LAPOR")
        self.start_btn.clicked.connect(self.launch)
        layout.addWidget(self.start_btn)

        self.setLayout(layout)

    def launch(self):
        endpoint = self.endpoint.text()
        target_id = self.target.text()
        reason = self.reason.text()
        token = self.token.text()
        try:
            total = int(self.total.text())
        except:
            total = 1

        threading.Thread(target=start_reporting, args=(endpoint, target_id, reason, token, total)).start()

# == Eksekusi Aplikasi ==
if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = ReportApp()
    window.show()
    sys.exit(app.exec_())
    

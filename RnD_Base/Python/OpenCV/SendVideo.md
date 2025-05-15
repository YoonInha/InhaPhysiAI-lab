연관 : [[ReciveVideo]]

개요 : 카메라가 연결된 리눅스 Opencv에서 다른 컴퓨터의 Opencv로 이미지 전송

```
# raspberry_sender.py
import cv2
import socket
import struct
import pickle

# 소켓 설정
server_ip = '192.168.0.2'  # 윈도우 PC의 IP
port = 9999
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect((server_ip, port))
connection = client_socket.makefile('wb')

# 비디오 캡처 시작
cap = cv2.VideoCapture(0)  # 필요 시 0 대신 /dev/video0 등 지정

try:
    while True:
        ret, frame = cap.read()
        if not ret:
            break
        # 프레임을 직렬화 (JPEG 압축 후 Pickle 직렬화)
        data = pickle.dumps(cv2.imencode('.jpg', frame)[1])
        size = len(data)
        # 사이즈를 먼저 전송한 후 데이터 전송
        client_socket.sendall(struct.pack(">L", size) + data)

except KeyboardInterrupt:
    pass

cap.release()
client_socket.close()

```
연관 : [[SendVideo]]
### CV환경에서 영상을 수신하여 출력

```
# windows_receiver.py
import socket
import cv2
import pickle
import struct

HOST = '0.0.0.0'
PORT = 9999

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind((HOST, PORT))
server_socket.listen(1)
print('Waiting for connection...')
conn, addr = server_socket.accept()
print(f'Connected by {addr}')

data = b""
payload_size = struct.calcsize(">L")

try:
    while True:
        while len(data) < payload_size:
            data += conn.recv(4096)

        packed_msg_size = data[:payload_size]
        data = data[payload_size:]
        msg_size = struct.unpack(">L", packed_msg_size)[0]

        while len(data) < msg_size:
            data += conn.recv(4096)

        frame_data = data[:msg_size]
        data = data[msg_size:]

        frame = pickle.loads(frame_data)
        frame = cv2.imdecode(frame, cv2.IMREAD_COLOR)

        # OpenCV 윈도우로 출력 (또는 영상 처리)
        cv2.imshow("Received", frame)
        if cv2.waitKey(1) == ord('q'):
            break

except KeyboardInterrupt:
    pass

conn.close()
cv2.destroyAllWindows()

```



### CV환경과 YOLO 모델을 이용한 객체 인식

```
import socket
import cv2
import pickle
import struct
from ultralytics import YOLO

# YOLOv8n (nano) 모델 로드 - 처음 실행 시 자동 다운로드
model = YOLO("yolov8n.pt")

# ===== 영상 수신 소켓 설정 =====
HOST = '0.0.0.0'
PORT = 9999

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind((HOST, PORT))
server_socket.listen(1)
print('Waiting for connection...')
conn, addr = server_socket.accept()
print(f'Connected by {addr}')

data = b""
payload_size = struct.calcsize(">L")

try:
    while True:
        while len(data) < payload_size:
            data += conn.recv(4096)

        packed_msg_size = data[:payload_size]
        data = data[payload_size:]
        msg_size = struct.unpack(">L", packed_msg_size)[0]

        while len(data) < msg_size:
            data += conn.recv(4096)

        frame_data = data[:msg_size]
        data = data[msg_size:]

        frame = pickle.loads(frame_data)
        frame = cv2.imdecode(frame, cv2.IMREAD_COLOR)

        # YOLOv8 객체 인식 수행
        results = model.predict(source=frame, conf=0.5, verbose=False)

        # bounding box와 label 출력
        for r in results:
            for box in r.boxes:
                x1, y1, x2, y2 = map(int, box.xyxy[0])
                cls_id = int(box.cls[0])
                conf = float(box.conf[0])
                label = f"{model.names[cls_id]} {conf:.2f}"
                cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
                cv2.putText(frame, label, (x1, y1 - 10), cv2.FONT_HERSHEY_SIMPLEX,
                            0.5, (0, 255, 0), 2)

        # 결과 화면 출력
        cv2.imshow("YOLOv8 Detection", frame)
        if cv2.waitKey(1) == ord('q'):
            break

except KeyboardInterrupt:
    pass

conn.close()
cv2.destroyAllWindows()


```
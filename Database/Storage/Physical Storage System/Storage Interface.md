---
상위 링크: "[[Physical Storage System]]"
---
# Storage Interface
Storage Interface는 저장장치와 컴퓨터 간의 데이터 전송 방식을 결정하는 규약이다.

## SATA
직렬 ATA (SATA) 인터페이스는 일반적인 소비자용 저장장치에서 가장 널리 사용되는 인터페이스이다. 데이터를 전송하는데에 직렬*Seraial* 방식을 사용한다. SATA-3은 명목 상 6GB/sec의 전송속도를 지원한다고 나와있지만, 실질적인 대역폭은 600MB/sec 가 된다. 

HDD에는 적합하지만, 고속 SSD를 사용할 때에는 낮은 대역폭으로 인해 하드웨어의 성능을 끌어내지 못한다.

## SAS
직렬 부착 SCSI(Serial Attached SCSI, SAS) 인터페이스는 서버 및 엔터프라이즈 스토리지에 사용되는 고성능 인터페이스이다. SAS 3버전은 12GB/sec을 지원한다.

## NVMe
Non-Volatile Memory Express 인터페이스는 **SSD를 더 잘 지원하기 위해 개발한 논리 인터페이스 표준**이며, 일반적으로 PCIe 인터페이스(컴퓨터의 마더보드와 다양한 하드웨어를 연결하는 고속 인터페이스)와 함께 사용된다. PCIe 5.0 x4 세대를 사용한다면 약 15.7 GB/sec의 전송속도를 지원한다.
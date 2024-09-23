`DW1000.h` 파일에서 **조절할 수 있는 파라미터**는 UWB 모듈(DW1000)의 **동작 모드, 통신 설정, 송신 전력, 주파수, 데이터 속도** 등과 관련된 파라미터들입니다. 이를 적절히 설정하면 **통신 거리, 전력 소모, 정확도** 등을 조정할 수 있습니다. 여기에서 직접 조절 가능한 주요 파라미터들을 설명드리겠습니다.

### **1. 통신 속도 설정**

통신 속도는 데이터를 얼마나 빠르게 전송할지 결정하는 중요한 요소입니다. DW1000에서는 세 가지 전송 속도를 지원합니다.

```cpp
// 통신 속도 설정 (110 kbps, 850 kbps, 6.8 Mbps)
static constexpr byte TRX_RATE_110KBPS  = 0x00; // 110 kbps
static constexpr byte TRX_RATE_850KBPS  = 0x01; // 850 kbps
static constexpr byte TRX_RATE_6800KBPS = 0x02; // 6.8 Mbps
```

- **설정 방법**: `DW1000.setDataRate(byte rate)`을 사용하여 설정할 수 있습니다.
  ```cpp
  DW1000.setDataRate(DW1000.TRX_RATE_6800KBPS); // 6.8 Mbps로 설정
  ```

### **2. 주파수 채널 설정**

DW1000 모듈은 6개의 채널에서 동작하며, 각 채널은 다른 중심 주파수에서 동작합니다. 이를 통해 통신 범위를 조절할 수 있습니다.

```cpp
// 사용 가능한 주파수 채널
static constexpr byte CHANNEL_1 = 1; // 3494.4 MHz
static constexpr byte CHANNEL_2 = 2; // 3993.6 MHz
static constexpr byte CHANNEL_3 = 3; // 4492.8 MHz
static constexpr byte CHANNEL_4 = 4; // 3993.6 MHz
static constexpr byte CHANNEL_5 = 5; // 6489.6 MHz
static constexpr byte CHANNEL_7 = 7; // 6489.6 MHz
```

- **설정 방법**: `DW1000.setChannel(byte channel)`을 사용하여 설정할 수 있습니다.
  ```cpp
  DW1000.setChannel(DW1000.CHANNEL_5); // 채널 5로 설정 (6.5GHz)
  ```

### **3. 송신 전력 설정**

송신 전력은 DW1000의 신호 강도를 조절하여 통신 거리를 제어합니다. 높은 송신 전력은 통신 거리를 늘리지만, 전력 소비도 증가합니다.

- **설정 방법**: `DW1000.setTXPower(uint32_t power)`를 사용하여 송신 전력을 설정할 수 있습니다. 이는 직접 설정할 수 있는 레지스터 값이며, 하드웨어 사양에 따라 적절한 값을 설정해야 합니다.

```cpp
DW1000.setTXPower(0x0E082848); // 송신 전력 설정 예시
```

### **4. 펄스 반복 주파수 (PRF) 설정**

펄스 반복 주파수(PRF)는 송수신 신호의 품질과 관련이 있습니다. 두 가지 PRF 설정이 있으며, 64MHz는 더 높은 전력 소비와 더 나은 성능을 제공합니다.

```cpp
// 사용 가능한 PRF
static constexpr byte TX_PULSE_FREQ_16MHZ = 0x01; // 16 MHz (저전력)
static constexpr byte TX_PULSE_FREQ_64MHZ = 0x02; // 64 MHz (고성능)
```

- **설정 방법**: `DW1000.setPulseFrequency(byte freq)`를 사용하여 설정합니다.
  ```cpp
  DW1000.setPulseFrequency(DW1000.TX_PULSE_FREQ_64MHZ); // 64 MHz PRF 설정
  ```

### **5. 프레임 길이 설정**

프레임 길이는 송수신할 데이터의 크기를 설정합니다. 짧은 프레임은 빠른 통신을 가능하게 하지만, 긴 프레임은 더 많은 데이터를 전송할 수 있습니다.

```cpp
// 프레임 길이 설정
static constexpr byte FRAME_LENGTH_NORMAL   = 0x00; // 일반 프레임
static constexpr byte FRAME_LENGTH_EXTENDED = 0x03; // 확장된 프레임
```

- **설정 방법**: 특정 프레임 길이를 설정하려면 관련 레지스터를 설정하거나 `DW1000.useExtendedFrameLength(boolean val)`을 사용할 수 있습니다.

```cpp
DW1000.useExtendedFrameLength(true); // 확장된 프레임 길이 사용
```

### **6. 프리앰블 길이 설정**

프리앰블은 통신 시작 시 송수신 간의 동기를 맞추기 위해 사용하는 신호로, 프리앰블 길이가 길수록 더 긴 거리를 측정할 수 있습니다.

```cpp
// 사용 가능한 프리앰블 길이
static constexpr byte TX_PREAMBLE_LEN_64   = 0x01;
static constexpr byte TX_PREAMBLE_LEN_128  = 0x05;
static constexpr byte TX_PREAMBLE_LEN_256  = 0x09;
static constexpr byte TX_PREAMBLE_LEN_512  = 0x0D;
static constexpr byte TX_PREAMBLE_LEN_1024 = 0x02;
static constexpr byte TX_PREAMBLE_LEN_1536 = 0x06;
static constexpr byte TX_PREAMBLE_LEN_2048 = 0x0A;
static constexpr byte TX_PREAMBLE_LEN_4096 = 0x03;
```

- **설정 방법**: `DW1000.setPreambleLength(byte prealen)`을 사용하여 설정할 수 있습니다.
  ```cpp
  DW1000.setPreambleLength(DW1000.TX_PREAMBLE_LEN_2048); // 프리앰블 길이 2048 설정
  ```

### **7. 스마트 전력(Smart Power) 설정**

스마트 전력 모드는 DW1000의 전력 소비를 최적화하는 옵션입니다. 신호 강도가 강한 경우 전력 소비를 줄일 수 있습니다.

- **설정 방법**: `DW1000.useSmartPower(boolean smartPower)`로 설정합니다.

```cpp
DW1000.useSmartPower(true); // 스마트 전력 사용
```

### **8. 데이터 필터 사용**

거리 측정의 정확도를 높이기 위해 필터를 사용할 수 있습니다. 데이터 필터는 잡음을 줄여 신호를 안정적으로 수신할 수 있게 합니다.

- **설정 방법**: `DW1000Ranging.useRangeFilter(true)`를 사용하여 필터를 활성화할 수 있습니다.

```cpp
DW1000Ranging.useRangeFilter(true); // 거리 측정 필터 활성화
```

### **9. 안테나 지연 설정**

안테나 지연은 신호가 안테나를 통과할 때의 시간 지연을 보정하기 위한 설정입니다. 이 값은 정확한 거리 측정을 위해 필요합니다.

- **설정 방법**: `DW1000.setAntennaDelay(uint16_t value)`을 사용하여 설정할 수 있습니다.

```cpp
DW1000.setAntennaDelay(16384); // 안테나 지연 설정 예시
```

---

### **정리**

다음은 DW1000에서 조절할 수 있는 주요 파라미터 목록입니다:

1. **통신 속도**: `DW1000.setDataRate()`
2. **주파수 채널**: `DW1000.setChannel()`
3. **송신 전력**: `DW1000.setTXPower()`
4. **펄스 반복 주파수(PRF)**: `DW1000.setPulseFrequency()`
5. **프레임 길이**: `DW1000.useExtendedFrameLength()`
6. **프리앰블 길이**: `DW1000.setPreambleLength()`
7. **스마트 전력 모드**: `DW1000.useSmartPower()`
8. **데이터 필터**: `DW1000Ranging.useRangeFilter()`
9. **안테나 지연**: `DW1000.setAntennaDelay()`

이러한 파라미터를 적절히 조정하면 **통신 거리**, **전력 소모**, **신호 품질** 등을 최적화할 수 있습니다.
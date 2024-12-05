# LED Control with FreeRTOS on STM32

## Project Description
This project demonstrates multitasking with **FreeRTOS** on STM32 by controlling two LEDs (green and red) that blink at specific intervals. Access to a shared resource (`startFlag`) is managed using **critical sections** to ensure thread safety.

---

## Key Features
- **Multitasking with FreeRTOS**:
  - Green LED and red LED are controlled by separate tasks.
  - Tasks run with different priorities and delay intervals.
- **Critical Section Usage**:
  - Protects access to shared resources (`startFlag`) during multitasking.
- **GPIO Control**:
  - Uses STM32 GPIO for LED toggling.

---

## Hardware Requirements
- STM32 microcontroller (e.g., STM32F103).
- Two LEDs (green and red) with resistors.
- Power supply and ST-Link programmer for STM32.

---

## GPIO Configuration
| LED         | GPIO Port | GPIO Pin |
|-------------|-----------|----------|
| Green LED   | PA0       | PIN 0    |
| Red LED     | PA2       | PIN 2    |

---

## Software Setup
1. Install **STM32CubeIDE**.
2. Ensure **FreeRTOS** is enabled in STM32CubeMX.
3. Import the project into STM32CubeIDE.
4. Build and upload the firmware to the STM32 board.

---

## Expected Behavior
- **Green LED**:
  - Turns on and off every 500 ms.
  - Accesses the shared resource (`startFlag`) inside a critical section.
- **Red LED**:
  - Turns on and off every 100 ms.
  - Accesses the shared resource without using a critical section.

---

## Code Overview
### Critical Section Implementation
```c
void GreenLedTask(void const * argument)
{
  for(;;)
  {
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_SET);
    
    taskENTER_CRITICAL();  // Enter critical section
    accessSharedData();
    taskEXIT_CRITICAL();   // Exit critical section
    
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_RESET);
    osDelay(500);
  }
}

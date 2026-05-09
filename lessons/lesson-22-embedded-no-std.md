# Урок 22. Embedded и no_std

## Теория

### Что такое no_std

`#![no_std]` отключает стандартную библиотеку, оставляя только `core` (без аллокатора, без OS-зависимых типов: нет `String`, `Vec`, файлов, сети). Используется в:

- микроконтроллерах (ARM Cortex-M, RISC-V)
- bootloader/firmware
- ядрах ОС
- WebAssembly без рантайма

```rust
#![no_std]
#![no_main]

use core::panic::PanicInfo;

#[panic_handler]
fn panic(_info: &PanicInfo) -> ! { loop {} }
```

### core / alloc / std

| Crate | Что входит | Где доступно |
|-------|-----------|--------------|
| core | примитивы, итераторы, Option/Result | везде |
| alloc | Box, Vec, String, Rc | если есть аллокатор |
| std | I/O, файлы, треды | только при наличии ОС |

Чтобы использовать `Vec` без std:

```rust
#![no_std]
extern crate alloc;
use alloc::vec::Vec;
```

### Цели сборки

```bash
rustup target add thumbv7em-none-eabihf
cargo build --target thumbv7em-none-eabihf
```

`thumbv7em-none-eabihf` — Cortex-M4F (STM32F4, nRF52). `riscv32imc-unknown-none-elf` — RISC-V.

### embedded-hal

`embedded-hal` — стандартный набор трейтов для драйверов: GPIO, SPI, I2C, UART, таймеры. Драйверы пишутся generic над трейтами, поэтому работают на любом MCU.

```rust
use embedded_hal::digital::OutputPin;

pub fn blink<P: OutputPin>(pin: &mut P) -> Result<(), P::Error> {
    pin.set_high()?;
    pin.set_low()?;
    Ok(())
}
```

### HAL и PAC

| Уровень | Что это |
|---------|---------|
| PAC | Peripheral Access Crate — низкоуровневые регистры, генерируется из SVD |
| HAL | High-level API (`stm32f4xx-hal`, `nrf52840-hal`, `rp2040-hal`) |
| BSP | Board Support Package — пресеты для конкретной платы |

### cortex-m-rt и точка входа

```rust
#![no_std]
#![no_main]
use cortex_m_rt::entry;

#[entry]
fn main() -> ! {
    loop {}
}
```

### Прерывания

```rust
use cortex_m_rt::exception;

#[exception]
fn SysTick() {
    // вызывается каждый системный тик
}
```

### RTIC и Embassy

- RTIC — приоритетные задачи без ОС, без аллокаций.
- Embassy — async/await на MCU, executor поверх таймеров и прерываний.

```rust
#[embassy_executor::main]
async fn main(_s: Spawner) {
    let mut led = ...;
    loop {
        led.set_high();
        Timer::after_millis(500).await;
        led.set_low();
        Timer::after_millis(500).await;
    }
}
```

### Глобальный аллокатор

```rust
use embedded_alloc::Heap;
#[global_allocator] static HEAP: Heap = Heap::empty();

fn init_heap() {
    use core::mem::MaybeUninit;
    const HEAP_SIZE: usize = 1024;
    static mut MEM: [MaybeUninit<u8>; HEAP_SIZE] = [MaybeUninit::uninit(); HEAP_SIZE];
    unsafe { HEAP.init(MEM.as_ptr() as usize, HEAP_SIZE) }
}
```

## Практика

### 1. Минимальная no_std программа (host)

```rust
#![no_std]
#![no_main]
use core::panic::PanicInfo;

#[no_mangle]
pub extern "C" fn _start() -> ! { loop {} }

#[panic_handler]
fn panic(_: &PanicInfo) -> ! { loop {} }
```

### 2. Blink на STM32F4 (HAL)

```rust
#![no_std] #![no_main]
use cortex_m_rt::entry;
use stm32f4xx_hal::{pac, prelude::*};

#[entry]
fn main() -> ! {
    let dp = pac::Peripherals::take().unwrap();
    let gpioc = dp.GPIOC.split();
    let mut led = gpioc.pc13.into_push_pull_output();
    let rcc = dp.RCC.constrain();
    let clocks = rcc.cfgr.freeze();
    let mut delay = dp.TIM1.delay_ms(&clocks);
    loop {
        led.toggle();
        delay.delay_ms(500u16);
    }
}
```

### 3. Driver-крейт

```rust
#![no_std]
use embedded_hal::i2c::I2c;

pub struct Bme280<I> { i2c: I, addr: u8 }

impl<I: I2c> Bme280<I> {
    pub fn new(i2c: I, addr: u8) -> Self { Self { i2c, addr } }

    pub fn read_temp(&mut self) -> Result<i32, I::Error> {
        let mut buf = [0u8; 3];
        self.i2c.write_read(self.addr, &[0xFA], &mut buf)?;
        Ok(((buf[0] as i32) << 12) | ((buf[1] as i32) << 4) | ((buf[2] as i32) >> 4))
    }
}
```

### 4. heapless::Vec — без аллокатора

```rust
use heapless::Vec;
let mut buf: Vec<u8, 16> = Vec::new();
buf.push(1).unwrap();
buf.push(2).unwrap();
```

### 5. Defmt-логирование

```rust
use defmt_rtt as _;
use defmt::info;

info!("temp = {}", 25);
```

### 6. Async на Embassy (RP2040)

```rust
#[embassy_executor::task]
async fn blinker(mut led: Output<'static, AnyPin>) {
    loop {
        led.set_high();
        Timer::after_millis(200).await;
        led.set_low();
        Timer::after_millis(800).await;
    }
}
```

## Тест

1. Что отключает `#![no_std]`?
2. В каком крейте лежат `Box`, `Vec`, `String` без std?
3. Что такое HAL и PAC?
4. Какая цель сборки для Cortex-M4F с FPU?
5. Зачем нужен `#[panic_handler]`?
6. Что делает `#[entry]` из `cortex-m-rt`?
7. Чем `embedded-hal` помогает разработчикам драйверов?
8. Что предлагает `Embassy`?
9. Зачем нужен `heapless`?
10. Что такое `defmt`?

---

### Ответы

1. Стандартную библиотеку `std` (нет ОС-зависимых типов и аллокатора).
2. `alloc` (нужен глобальный аллокатор).
3. PAC — низкоуровневый доступ к регистрам; HAL — высокоуровневое API поверх PAC.
4. `thumbv7em-none-eabihf`.
5. `no_std` не имеет встроенного обработчика паники, его нужно определить вручную.
6. Помечает функцию как точку входа после reset для Cortex-M.
7. Даёт стандартные трейты (GPIO/I2C/SPI/...), которые позволяют писать переносимые драйверы.
8. Async/await runtime для embedded — задачи, таймеры, async-драйверы.
9. Коллекции с фиксированной capacity на стеке без аллокатора.
10. Эффективный фреймворк логирования: формат форматируется на хосте, а на устройстве отправляются только индексы и аргументы.

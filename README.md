# UART Verilog Modules

This repository includes Verilog modules for a Universal Asynchronous Receiver-Transmitter (UART) system, enabling serial communication with a configurable baud rate. The key components are a baud rate generator, receiver, and a top-level UART module.

## Table of Contents
- [Modules Overview](#modules-overview)
  - [baud_rate_gen.v](#baud_rate_genv)
  - [receiver.v](#receiverv)
  - [uart.v](#uartv)
- [Usage](#usage)
- [License](#license)

---

## Modules Overview

### `baud_rate_gen.v`
The `baud_rate_gen` module is responsible for generating clock enable signals (`rxclk_en` and `txclk_en`) to control the baud rate for both receiving and transmitting data. This module divides a 50 MHz input clock down to generate a 115200 baud rate signal, with the receiver clock oversampling at 16x for enhanced accuracy.

#### Ports
- `clk_50m` (input): 50 MHz input clock.
- `rxclk_en` (output): Clock enable for the receiver, oversampled at 16x.
- `txclk_en` (output): Clock enable for the transmitter, aligned with the desired baud rate.

---

### `receiver.v`
The `receiver` module manages the UART receiving process, storing each received byte and setting a "ready" (`rdy`) flag when data is available.

#### Parameters and States
- **States**:
  - `RX_STATE_START`: Detects the start of a transmission.
  - `RX_STATE_DATA`: Captures data bits.
  - `RX_STATE_STOP`: Verifies stop bits.

#### Ports
- `rx` (input): Serial data input.
- `rdy` (output): Indicates when a full byte is received.
- `rdy_clr` (input): Clears the `rdy` flag once data is read.
- `clk_50m` (input): 50 MHz clock.
- `clken` (input): Enable signal from the baud rate generator.
- `data` (output): 8-bit data output from the receiver.

---

### `uart.v`
The top-level `uart` module integrates the baud rate generator, transmitter, and receiver modules. It facilitates bidirectional communication by handling input (`din`) and output (`dout`) data bytes.

#### Ports
- `din` (input): 8-bit data to transmit.
- `wr_en` (input): Write enable for transmission.
- `clk_50m` (input): 50 MHz clock.
- `tx` (output): Serial data output.
- `tx_busy` (output): Indicates if the transmitter is busy.
- `rx` (input): Serial data input.
- `rdy` (output): Indicates if received data is ready.
- `rdy_clr` (input): Clears the `rdy` flag once data is read.
- `dout` (output): 8-bit data output from the receiver.

---

## Usage
1. Connect the `clk_50m` input to a 50 MHz clock source.
2. Interface with the `uart` module for sending and receiving 8-bit data.
3. Use `wr_en` to initiate transmission and monitor `tx_busy` for transmission status.
4. Check `rdy` to know when received data is ready and read `dout` for the received byte.

## License
This project is open-source. You are free to use, modify, and distribute these modules in your projects.

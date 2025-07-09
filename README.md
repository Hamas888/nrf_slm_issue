# nrf_slm_issue

Here is a step-by-step guide for setting up, building, flashing, and testing the Serial LTE Modem (SLM) application with trace/log output enabled on the nRF9160 DK using nRF Connect SDK v2.7.0:

---

# Serial LTE Modem (SLM) Setup, Build, Flash, and Trace Guide (nRF9160 DK, NCS v2.7.0)

## 1. Install nRF Connect for Desktop

- Download and install **nRF Connect for Desktop** from:  
  https://www.nordicsemi.com/Products/Development-tools/nrf-connect-for-desktop

## 2. Install Toolchain Manager

- Open **nRF Connect for Desktop**.
- Go to the **Toolchain Manager** tab and install it if not already present.

## 3. Install nRF Connect SDK v2.7.0

- In **Toolchain Manager**, find and install **nRF Connect SDK v2.7.0**.
- This will install the SDK, toolchain, and required tools.

## 4. Install the nRF Connect SDK Extension for VS Code

- In **nRF Connect for Desktop**, go to the **VS Code Extension** tab.
- Install the **nRF Connect SDK extension** for Visual Studio Code.

## 5. Open the Project in VS Code

- In **Toolchain Manager**, click **Open VS Code** for the nRF Connect SDK v2.7.0 environment.
- In VS Code, open the folder:  
  serial_lte_modem

## 6. Make Configuration Changes for Trace/Log Output

- Open `prj.conf` in the SLM application directory.
- Ensure the following lines are set (they already are in your config):
  ```
  CONFIG_LOG=y
  CONFIG_LOG_BACKEND_UART=y
  CONFIG_UART_CONSOLE=y
  CONFIG_USE_SEGGER_RTT=n
  CONFIG_LOG_BACKEND_RTT=n
  CONFIG_RTT_CONSOLE=n
  ```
- Save the file.

## 7. Build the Application

- Open a terminal in VS Code or use the built-in terminal in Toolchain Manager.
- Navigate to the SLM application directory:
  ```powershell
  cd c:\ncs\v2.7.0\nrf\applications\serial_lte_modem
  ```
- Build for the nRF9160 DK (non-secure):
  ```powershell
  west build -b nrf9160dk/nrf9160/ns --pristine
  ```

## 8. Flash the Application

- Connect your nRF9160 DK to your PC via USB.
- Flash the firmware:
  ```powershell
  west flash
  ```

## 9. View Traces and Logs

- Open **nRF Connect for Desktop** and launch the **Serial Terminal** app, or use a terminal program like PuTTY or Tera Term.
- Select the correct COM port for your DK (check Device Manager).
- Set the baud rate to **115200**, 8 data bits, no parity, 1 stop bit, no flow control.
- Reset or power cycle the DK if needed.
- You should see log and trace output from the SLM application.

---

## Summary of Key Commands

```powershell
# Navigate to the SLM application directory
cd c:\ncs\v2.7.0\nrf\applications\serial_lte_modem

# Build the application
west build -b nrf9160dk/nrf9160/ns --pristine

# Flash the application
west flash
```

---

**Tip:**  
If you do not see logs, double-check your UART/COM port connection and terminal settings.

Let me know if you need a PDF or markdown file for this guide!

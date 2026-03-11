# ameba-rtos-z2
GitHub `ameba-rtos-z2` repository is the development framework for AmebaZ2 and AmebaZ2Plus SOCs.

# Supported SoCs

|Chip         |                Project                 |
|-------------|----------------------------------------|
|AmebaZ2      |/project/realtek_amebaz2_v0_example     |
|AmebaZ2Plus  |/project/realtek_amebaz2plus_v0_example |

# Documentation
* :books: [Application Notes](https://github.com/Ameba-AIoT/ameba-rtos-z2/blob/main/doc/AN0500_Realtek_Ameba-ZII_Application_Note.pdf)
* :books: [Z2 Datasheet](https://github.com/Ameba-AIoT/ameba-rtos-z2/blob/main/doc/RTL8720Cx-VH2_Datasheet_V1.0_20230224.pdf)
* :books: [Z2plus Datasheet](https://github.com/Ameba-AIoT/ameba-rtos-z2/blob/main/doc/Realtek_AmebaZII+_Datasheet_v1.1.pdf)

# Matter Support
Please check out [ameba-rtos-matter](https://github.com/Ameba-AIoT/ameba-rtos-matter) to learn more about Matter support.

# Setup Build Environments

## GCC Environment on Windows
On Windows, you can use Cygwin as the GCC environment. Cygwin is a large collection of GNU and Open Source tools which provide the similar functionality as a Linux distribution on Windows. Please visit the official website of Cygwin and install the software.

Note:
* Please use Cygwin 32-bit version.
* During the Cygwin installation, please install "math -> bc" and "devel -> make".

## GCC Environment on Linux
On 64-bit Linux, please install packages for the GCC environment.
* Use command `apt-get install make` to install "make".
* Use command `apt-get install bc` to install "bc".

# Configure the Project
* For general configurations, you can configure the options in "platform_opts.h".
* For BT configurations, you can configure the options in "platform_opts_bt.h".

# Compile the Project
1) Open the Cygwin Terminal or Linux terminal.
2) Direct to compile path. Execute command `cd /project/.../GCC-RELEASE`.
3) Clean up pervious compilation files. Execute command `make clean`.
4) Build the SDK. Execute command `make all`.
5) Make sure there is no error after compilation.

# Download image
After a successfully compilation, the images `partition.bin`, `bootloader.bin`, `firmware_is.bin` and `flash_is.bin` can be seen in the folder/sub-folder of /GCC-RELEASE.
* `partition.bin` stores partition table, recording the address of Boot image and firmware image;
* `bootloader.bin` is bootloader image;
* `firmware_is.bin` is application image;
* `flash_is.bin` links `partition.bin`, `bootloader.bin` and `firmware_is.bin`.

To download image to board, you can either
* Directly download the image binary to board from GCC (J-Link debugger is required), please check the ApplicationNote chapter **SDK Build Environment Setup** for more details.
* Or using the PG tool for Ameba-ZII (in /tools/AmebaZ2), please check the ApplicationNote chapter **Image Tool** for more details.

**Note**: Please choose `flash_is.bin` when downloading image by PG Tool.

# CI 自動編譯說明（中文）

本次針對 `.github/workflows/main.yml` 進行了以下修改，目的是讓 GitHub Actions 能夠自動編譯並產生 AmebaZ2 / AmebaZ2Plus 的韌體映像檔：

1. **新增安裝編譯相依套件的步驟**：在編譯前先執行 `sudo apt-get install -y bc python-is-python3`，安裝編譯腳本所需的 `bc`（數學運算工具）及 `python-is-python3`（確保 `python` 指令可在新版 Ubuntu 上正常執行）。

2. **移除編譯後的 `make clean`**：原本的工作流程在編譯完成後會立即執行 `make clean` 清除產出物，導致映像檔無法被收集。移除此步驟後，映像檔得以保留供後續上傳。

3. **上傳映像檔**：編譯完成後，透過 `actions/upload-artifact@v4` 將以下四個映像檔分別上傳為 GitHub Actions Artifact，供下載使用：
    - `partition.bin`：分割表映像檔
    - `bootloader.bin`：開機載入程式映像檔
    - `firmware_is.bin`：應用程式映像檔
    - `flash_is.bin`：包含上述三者的完整快閃記憶體映像檔

    AmebaZ2 與 AmebaZ2Plus 兩個專案均執行此上傳步驟。

4. **設定 `if-no-files-found: error`**：若預期的映像檔不存在（例如編譯失敗），上傳步驟會明確報錯，方便快速定位問題。

# Release Notes

## Vesion Sync
GitHub `ameba-rtos-z2` is currently synchronized with 7.1 GIT 20251103_7eba7db1.

# Flash Attention Windows Wheels

Pre-built Windows wheels for [Flash-Attention 2](https://github.com/Dao-AILab/flash-attention) - The state-of-the-art efficient attention implementation for NVIDIA GPUs.

## Overview 

This repository was created in response to the numerous challenges Windows users face when trying to build Flash-Attention 2. 
After experiencing these difficulties firsthand and seeing many similar issues in the official repository (#1340, #1339, #1292), it became clear that a solution was needed to make this cutting-edge attention implementation immediately accessible in Windows environments.

Building Flash-Attention on Windows requires a delicate setup process with specific versions of multiple dependencies, careful environment configuration, and significant time investment. This repository eliminates these hurdles by providing ready-to-use pre-built wheels, making this cutting-edge attention implementation immediately accessible in Windows environments without complex and time-consuming build requirements.

Flash Attention 2, originally created by Tri Dao and Dan Fu, delivers breakthrough performance:
- 20% faster than Flash Attention 1 
- Up to 10x faster than standard attention
- Up to 20x memory reduction
- Drop-in replacement for PyTorch's attention
- Automatic CUDA kernel optimization

These wheels are tested and maintained to ensure stable deployment on Windows, saving hours of build configuration and potential compatibility issues. While not officially supported by the Flash-Attention team, they are built following the exact source specifications to maintain the original performance benefits.

## Why These Wheels?

1. *Solves Common Build Issues**: Addresses widespread Windows build failures documented in multiple GitHub issues
2. **Eliminates Build Complexity**: Bypasses the need for precise configuration of Visual Studio, CUDA toolkit, and build environment
3. **Saves Hours of Development Time**: Replaces a fragile, time-consuming build process with simple pip installation for Immediate integration into your PyTorch projects
4. **Verified Functionality**: Tested against standard benchmarks to ensure performance
5. **Regular Updates**: Maintained to keep pace with Flash Attention releases
6. **Community Support**: Issues and improvements handled via GitHub collaboration

Note: These wheels are community-maintained and are not officially supported by the Flash-Attention team. They are provided to support the ML community's Windows developers.

## Available Wheels

### Python 3.12 / CUDA 12.1 / PyTorch 2.5.1

* **Flash Attention Version:** `2.7.4.post1`
* **Wheel File:** `wheels/py312_cu121_torch251/flash_attn-2.7.4.post1-cp312-cp312-win_amd64.whl`
* **Python Version:** `3.12.x` (`cp312`)
* **PyTorch Version:** `2.5.1+cu121`
* **CUDA Toolkit Version:** `12.1`
* **Platform:** Windows 10/11 (x64)
* **Build Date:** April 2025
* **Build Context:** Built using VS 2022 v17.4.x LTSC toolchain.

### Python 3.10 / CUDA 11.7+ / PyTorch 2.0.0+

* **Flash Attention Version:** `2.7.0.post2`
* **Wheel File:** `flash_attn-2.7.0.post2-cp310-cp310-win_amd64.whl` *(Assuming it's in the root or adjust path)*
* **Python Version:** `3.10` (`cp310`)
* **CUDA Toolkit Version:** `11.7+`
* **PyTorch Version:** `2.0.0+`
* **Platform:** Windows 10/11 (x64)
* **Build Date:** November 2024

## Requirements

Ensure your environment meets the prerequisites for the specific wheel you intend to use:

**For Python 3.12 Wheel (`flash_attn-2.7.4.post1`):**

* Windows 10/11 (64-bit)
* **Python 3.12.x**
* **CUDA Toolkit 12.1** installed system-wide.
* **PyTorch 2.5.1 built for CUDA 12.1 (`torch==2.5.1+cu121`)**. Install with:
    ```bash
    pip install torch==2.5.1 torchvision torchaudio --index-url [https://download.pytorch.org/whl/cu121](https://download.pytorch.org/whl/cu121)
    ```
* NVIDIA GPU with Compute Capability 8.0+ (Ampere, Ada, Hopper). Minimum 8GB VRAM recommended.

**For Python 3.10 Wheel (`flash_attn-2.7.0.post2`):**

* Windows 10/11 (64-bit)
* **Python 3.10**
* **CUDA Toolkit 11.7+** installed system-wide.
* **PyTorch 2.0.0+**
* NVIDIA GPU with Compute Capability 8.0+. Minimum 8GB VRAM recommended.

## Installation

1.  Ensure your environment meets the specific [Requirements](#requirements) for the wheel you intend to use.
2.  Activate your target Python environment.
3.  Go to the **[Releases](https://github.com/sunsetcoder/flash-attention-windows/releases)** page of this repository.
4.  Download the appropriate `.whl` file for your environment from the release assets listed for a specific release tag.
5.  Install the downloaded wheel using pip (replace `<path_to_downloaded_wheel_file.whl>` with the actual path and filename where you downloaded it):
    ```bash
    pip install <path_to_downloaded_wheel_file.whl> --no-build-isolation --no-deps
    ```
    *(Or use `python -m pip install ...` if you have multiple Python versions).*
    *(Note: Using `--no-deps` assumes PyTorch and other prerequisites are already installed correctly as per the Requirements section).*

**Example:**
```sh
# Simply download the wheel file and install with:
pip install flash_attn-2.7.0.post2-cp310-cp310-win_amd64.whl
```

## Verification

```python
try:
    import torch
    import flash_attn
    from flash_attn import flash_attn_func
    
    # Verify version
    print(f"Flash Attention version: {flash_attn.__version__}")
    
    # Basic functionality test
    if torch.cuda.is_available():
        q = torch.randn(2, 8, 32, 64, device='cuda')
        k = torch.randn(2, 8, 32, 64, device='cuda')
        v = torch.randn(2, 8, 32, 64, device='cuda')
        
        output = flash_attn_func(q, k, v)
        print("Flash Attention test successful!")
    else:
        print("CUDA device not available!")
except ImportError as e:
    print(f"Import Error: {e}")
except RuntimeError as e:
    print(f"Runtime Error: {e}")
```

## Known Issues

- Building `flash-attn` on Windows _may_ require specific Visual Studio versions due to CUDA compatibility (e.g., the CUDA 12.1 build was successful with VS 2022 17.4.x LTSC when newer VS versions failed). Running pre-built wheels does not require VS to be installed.

## Instructions for Building New Wheels

These instructions provide examples based on successful builds reported by contributors. Building `flash-attn` on Windows is complex and highly sensitive to specific version combinations of the CUDA Toolkit, PyTorch, and Visual Studio C++ toolchain.

### Example Build: Py3.12 / CUDA 12.1 / PyTorch 2.5.1 (Yielding `flash-attn==2.7.4.post1`)

This process was successfully used on Windows 11 with an NVIDIA RTX 4070 (12 GB) and 32 GB RAM.

#### Prerequisites

* **Visual Studio:** **VS 2022 LTSC version 17.4.x**, installed optionally side-by-side (instructions below) with newer versions like 17.13+, which were found incompatible with CUDA 12.1 during this build.
    * Run this in a Command Prompt **as Admin** from the directory containing `VisualStudioSetup.exe`:  
  ```bash
  VisualStudioSetup.exe --channelUri https://aka.ms/vs/17/release.LTSC.17.4/channel
  ```  
    * Must install with the workloads option and include the "Desktop development with C++" workload.
* **CUDA Toolkit:** **12.1** (*not* newer!) installed system-wide.
* **Python:** **Python 3.12.x** (used within an Anaconda environment in this test).
* **PyTorch:** **`torch==2.5.1+cu121`** installed in the environment (along with compatible `torchvision`/`torchaudio`). Use:
    ```bash
    pip install torch==2.5.1 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
    ```
* **(Optional) Ninja:** Installing the `ninja` build system (`pip install ninja` or `python -m pip install ninja` with multiple Python versions) before starting the build may significantly speed up the compilation process, but it is not strictly required.

#### Build Steps

1.  **Launch Correct Prompt:** Open the specific **`x64 Native Tools Command Prompt for VS 2022 LTSC 17.4`** corresponding to the correct VS 2022 installation. *Do not use standard CMD/PowerShell or prompts from other VS versions.*
2.  **Activate Environment & Set Variables:**
    ```bash
    call D:\anaconda3\Scripts\activate.bat base  # Adjust Anaconda/env path if needed
    cd path\to\your\build\directory          # Navigate to where you want to build
    set DISTUTILS_USE_SDK=1                 # Crucial for using VS 2022 SDK
    set MAX_JOBS=2                          # Reduce to 1 if you encounter "out of memory" errors during compilation
    ```
3.  **Upgrade Build Tools & Install:**
    ```bash
    pip install --upgrade pip setuptools wheel
    pip install flash-attn --no-build-isolation   # This triggers the build
    ```

    Consider these commands if you have multiple Python versions installed:
    ```bash
    python -m pip install --upgrade pip setuptools wheel
    python -m pip install flash-attn --no-build-isolation   # This triggers the build
    ```
4.  **Wait:** Expect the build to potentially take 1–3+ hours. The compiled wheel will be stored in the pip cache, its exact location reported in the command prompt.

---

### Example Build: Py3.10 / CUDA 12.4 / PyTorch 2.0.0+ (Yielding `flash-attn==2.7.0.post2`)

This section reflects the build process originally documented for the Python 3.10 wheel.

#### Prerequisites

- Visual Studio 2019 with C++ build tools
- CUDA Toolkit 12.4
- Python 3.10 development environment
- Administrator privileges

#### Build Steps

1.  **Prepare Environment**
```sh
# Install build dependencies
pip install ninja packaging

# Set environment variables (PowerShell)
$env:FLASH_ATTENTION_FORCE_BUILD="TRUE"
$env:CUDA_HOME="C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.4"
```

2.  **Build Process**
```sh
# Remove existing installation
pip uninstall flash-attn -y

# Install with build flag
pip install flash-attn==2.7.0.post2 --no-build-isolation
```

#### Build Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| FLASH_ATTENTION_FORCE_BUILD | Forces wheel rebuild | FALSE |
| CUDA_HOME | CUDA toolkit path | System CUDA path |
| MAX_JOBS* | Build parallelism | CPU count |

* If your machine has less than 96GB of RAM and lots of CPU cores, ninja might run too many parallel compilation jobs that could exhaust the amount of RAM. To limit the number of parallel compilation jobs, you can set the environment variable MAX_JOBS (try 4 to start with).

## Troubleshooting

If you encounter issues building `flash-attn` based on the examples above, consider these common problems and solutions:

### Common Build Errors & Solutions (Py3.12 / CUDA 12.1 Build Example)

Before diving into specific errors, always double-check the fundamental prerequisites for the build you are attempting:

* Are you using the correct **Visual Studio Developer Command Prompt** (e.g., `x64 Native Tools Command Prompt for VS 2022 LTSC 17.4`)?
* Is the correct **CUDA Toolkit** version installed system-wide (e.g., 12.1)?
* Is the correct **PyTorch version** (e.g., `2.5.1+cu121`) installed in your *active* Python environment?
* Are the necessary **environment variables** (`DISTUTILS_USE_SDK`, `MAX_JOBS`) set correctly in your command prompt session?

**Specific Errors:**

* **Error:** `unsupported Microsoft Visual Studio version!` (often seen in CUDA `host_config.h` logs):
    * **Solution:** You are likely using a Visual Studio version incompatible with the specific CUDA Toolkit version required. For CUDA 12.1, downgrading VS 2022 to the **LTSC 17.4.x** version was necessary. Ensure you installed the correct LTSC version side-by-side and are using its specific Developer Command Prompt.
* **Error:** `'identifier not found'` (e.g., `_addcarry_u64`) or errors referencing `win32` paths during compilation:
    * **Solution:** This usually indicates you launched the build from the wrong command prompt. Ensure you are using the specific **`x64 Native Tools Command Prompt for VS 2022 LTSC 17.4`** (or the equivalent for your required VS version), not a standard Command Prompt, PowerShell, or a prompt from a different VS installation.
* **Error:** `cl.exe: catastrophic error: out of memory during compilation`:
    * **Solution:** The C++ compiler ran out of RAM while processing complex code.
        * Reduce build parallelism by setting the environment variable `set MAX_JOBS=2` or, if you already tried `2`, `set MAX_JOBS=1` before running `pip install`.
        * Ensure your system has adequate Virtual Memory (Page File) configured in Windows settings.
* **Error:** `failed building wheel for flash-attn` (Generic):
    * **Solution:** This is a general failure message. Check the detailed error logs preceding it. Common causes include the VS/CUDA incompatibility, incorrect command prompt, out-of-memory errors (see above), or missing C++ build components (ensure the "Desktop development with C++" workload was fully installed in VS). Setting `MAX_JOBS=1` can sometimes help pinpoint the underlying issue by simplifying the build process at the cost of it taking significantly longer.


### Common Issues (Py3.10 / CUDA 12.4 Build Example)

1. **Installation Failures**
   - Verify CUDA installation
   - Check Python version (`which python`)
   - Confirm VS2019 installation

## Contributing (General)

Contributions welcome in these areas:
- Documentation improvements
- Build process optimization
- Wheels for other versions of Python and Flash Attention.

## Contributing Wheels

We welcome contributions of pre-built wheels for other configurations! If you have successfully built a wheel for a different Python, CUDA, or PyTorch version combination on Windows, please consider sharing it:

1. **Fork** this repository.
2. **Add your wheel:** Create a subdirectory under `wheels/` using a clear naming convention (e.g., `py<VER>_cu<VER>_torch<VER>/`) and place your `.whl` file inside.
3. **Update `README.md`:** Add entries for your wheel under the "Available Wheels", "Requirements", and "Security" sections, following the existing format. Please include build context details if known (e.g., VS version used).
4. **Calculate Checksum:** Add the SHA256 checksum for your wheel under the "Security" section.
5. **Submit a Pull Request:** Create a Pull Request back to this repository with a clear title and description explaining your contribution.

## License

Distributed under the same license as Flash Attention. See [Flash Attention License](https://github.com/Dao-AILab/flash-attention/blob/main/LICENSE).

## Resources

- [Flash Attention Repository](https://github.com/Dao-AILab/flash-attention)
- [Flash Attention Paper](https://tridao.me/publications/flash2/flash2.pdf)
- [PyTorch Documentation](https://pytorch.org/)

## Support

- Star this repository
- Report issues
- Submit pull requests (wheels for other Python versions or new versions of Flash Attention)
- Share with others

## Security

Verify downloaded wheel checksums:

```powershell
# Generate checksum (Powershell)
Get-FileHash <WHEEL_FILENAME> -Algorithm SHA256
```

**Expected Values:**

- `Py3.12 / CUDA 12.1 / PyTorch 2.5.1 Wheel:` `B7E5750A9F1AA0E6BDB608B3AA860C83B6DC7552E1E74AD01B0B967D9F15F489 flash_attn-2.7.4.post1-cp312-cp312-win_amd64.whl`
- `Py3.10 / CUDA 11.7+ Wheel:` `15e0c4af6349b66c1003bf8541487636aca0a6ad81d6593d6711409983fd616c flash_attn-2.7.0.post2-cp310-cp310-win_amd64.whl`

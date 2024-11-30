```markdown
# Flash Attention Windows Wheels (Python 3.10)

Pre-built Windows wheels for [Flash-Attention 2](https://github.com/Dao-AILab/flash-attention) - The state-of-the-art efficient attention implementation for NVIDIA GPUs.

## Overview 

This repository provides Windows compatibility for Flash Attention 2 through ready-to-use pre-built wheels, making this cutting-edge attention implementation immediately accessible in Windows environments without complex and time-consuming build requirements.

Flash Attention 2, originally created by Tri Dao and Dan Fu, delivers breakthrough performance:
- 20% faster than Flash Attention 1 
- Up to 10x faster than standard attention
- Up to 20x memory reduction
- Drop-in replacement for PyTorch's attention
- Automatic CUDA kernel optimization

These wheels are tested and maintained to ensure stable deployment on Windows, saving hours of build configuration and potential compatibility issues. While not officially supported by the Flash-Attention team, they are built following the exact source specifications to maintain the original performance benefits.

## Why These Wheels?

1. **Eliminates Build Complexity**: Bypass the complex build environment setup required for Windows
2. **Saves Development Time**: Immediate integration into your PyTorch projects
3. **Verified Functionality**: Tested against standard benchmarks to ensure performance
4. **Regular Updates**: Maintained to keep pace with Flash Attention releases
5. **Community Support**: Issues and improvements handled via GitHub collaboration

Note: These wheels are community-maintained and are not officially supported by the Flash-Attention team. They are provided to support the ML community's Windows developers.

## Current Release

- Flash Attention Version: 2.7.0.post2
- Python Version: 3.10
- Platform: Windows 11 (x64) + NVIDIA GPU (Ada Lovelace architecture)
- Build Date: November 2024

## Requirements

- Windows 10/11 (64-bit)
- Python 3.10
- CUDA Toolkit 11.7+
- NVIDIA GPU with Compute Capability 8.0+. Compatible with Ampere, Ada, or Hopper GPUs (e.g., A100, RTX 3090, RTX 4090, H100).
- PyTorch 2.0.0+
- Minimum 8GB GPU VRAM recommended

## Quick Installation

```bash
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

- Wheels only available for Python 3.10. Python 3.12 support is in the roadmap.

## Instructions for building new wheels:

### Prerequisites

- Visual Studio 2019 with C++ build tools
- CUDA Toolkit 12.4
- Python 3.10 development environment
- Administrator privileges

### Build Steps

1. **Prepare Environment**
```powershell
# Install build dependencies
pip install ninja packaging

# Set environment variables (PowerShell)
$env:FLASH_ATTENTION_FORCE_BUILD="TRUE"
$env:CUDA_HOME="C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.4"
```

2. **Build Process**
```powershell
# Remove existing installation
pip uninstall flash-attn -y

# Install with build flag
pip install flash-attn==2.7.0.post2 --no-build-isolation
```

### Build Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| FLASH_ATTENTION_FORCE_BUILD | Forces wheel rebuild | FALSE |
| CUDA_HOME | CUDA toolkit path | System CUDA path |
| MAX_JOBS* | Build parallelism | CPU count |

* If your machine has less than 96GB of RAM and lots of CPU cores, ninja might run too many parallel compilation jobs that could exhaust the amount of RAM. To limit the number of parallel compilation jobs, you can set the environment variable MAX_JOBS (try 4 to start with).

## Troubleshooting

### Common Issues

1. **Installation Failures**
   - Verify CUDA installation
   - Check Python version (`which python`)
   - Confirm VS2019 installation

## Contributing

Contributions welcome in these areas:
- Documentation improvements
- Build process optimization
- Wheels for other versions of Python and Flash Attention.

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
```
# Generate checksum (Powershell)
Get-FileHash flash_attn-2.7.0.post2-cp310-cp310-win_amd64.whl -Algorithm SHA256

# Compare with expected value
15e0c4af6349b66c1003bf8541487636aca0a6ad81d6593d6711409983fd616c  flash_attn-2.7.0.post2-cp310-cp310-win_amd64.whl
```
# Isaac Lab Setup Fixes

## Issue 1: VSCode Setup Script Failed - Missing _isaac_sim Symlink

### Problem
```
FileNotFoundError: Could not find the isaac-sim directory: /home/njeai/IsaacLab/_isaac_sim
```

The setup script was looking for `_isaac_sim` symlink, but found a broken `isaacsim` symlink pointing to the unexpanded variable `${HOME}/isaacsim`.

### Solution
```bash
cd /home/njeai/IsaacLab
rm isaacsim
ln -s ~/isaacsim _isaac_sim
```

### Verification
```bash
ls -la _isaac_sim
# Should show: _isaac_sim -> /home/njeai/isaacsim
```

---

## Issue 2: Module Not Found - 'isaacsim' Package

### Problem
```
ModuleNotFoundError: No module named 'isaacsim'
```

The `isaacsim` Python module wasn't accessible from the conda environment.

### Solution
Install Isaac Lab dependencies and ensure proper environment setup:
```bash
cd /home/njeai/IsaacLab
./isaaclab.sh --install
```

---

## Issue 3: Library Conflicts - Exit Code 137 (Out of Memory/Killed)

### Problem
Script was killed (exit code 137) when running, potentially due to library conflicts between conda environment and system libraries.

### Solution
Install compatible libstdc++ in the conda environment:
```bash
conda activate env_isaaclab
conda install -c conda-forge libstdcxx-ng -y
```

---

## Complete Setup Workflow

To set up Isaac Lab from scratch:

1. **Create and activate conda environment:**
   ```bash
   conda create -n env_isaaclab python=3.10 -y
   conda activate env_isaaclab
   ```

2. **Fix symlink:**
   ```bash
   cd /home/njeai/IsaacLab
   rm isaacsim  # if broken symlink exists
   ln -s ~/isaacsim _isaac_sim
   ```

3. **Install dependencies:**
   ```bash
   conda install -c conda-forge libstdcxx-ng -y
   ./isaaclab.sh --install
   ```

4. **Setup VSCode:**
   ```bash
   ./isaaclab.sh -p .vscode/tools/setup_vscode.py
   ```

5. **Run test script:**
   ```bash
   ./isaaclab.sh -p scripts/tutorials/00_sim/create_empty.py
   ```

---

## Notes

- The `_isaac_sim` symlink must point to your Isaac Sim installation directory
- Always activate the conda environment before running Isaac Lab scripts
- The `isaaclab.sh` wrapper handles Python environment setup automatically
- Library conflicts can occur when conda's libraries conflict with Isaac Sim's bundled libraries

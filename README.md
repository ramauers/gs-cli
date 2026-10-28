# gs-cli

`gs-cli` is a lightweight Bash script designed to simplify the installation and management of **GemStone/S 64 Bit** environments. 

Originally created for personal use, this tool is now open-source and shared under the Public Domain for anyone who might find it useful.

## Why gs-cli?

GemStone/S 64 Bit is a fascinating, well-documented piece of technology, and working with it is highly rewarding. However, the official product does not ship with an automated setup tool or built-in scripts for fundamental database management tasks — such as starting, stopping, creating backups, or performing restores.

The goal of `gs-cli` is to bridge this gap by making the setup process as effortless as possible.

### Key Features
* **All-in-One Script:** Built entirely as a single, portable Bash file.
* **Smart Context Awareness:** If placed in an empty directory, it automatically triggers the installation workflow. If GemStone/S is already installed, it provides subcommands to manage your existing environment.
* **Focused Scope:** Streamlined specifically for essential database management tasks.
* **Fully Encapsulated Environment:** All necessary environment settings are managed internally by `gs-cli`. There is no need to define or export any GemStone-specific variables in your regular shell environment.

### Current Limitations
* **OS & Architecture:** Currently targets **Linux x86_64** only. Platform detection for Linux on ARM or macOS is not yet supported.
* **Golden Copy Extent:** GemStone ships with different empty extent options. The script currently defaults to the Seaside extent. To use a different one, manually change the variable `GSCLI_GOLDEN_COPY_EXTENT_NAME=extent0.seaside.dbf` inside the script.
* **Alternative Tooling:** For a more advanced and comprehensive management suite, check out Dale Henrichs' [GsDevKit_stones](https://github.com).

## Installation & Setup

To install GemStone/S using `gs-cli`, create an empty directory, download the script, and make it executable:

```bash
# Create and navigate to your project directory
mkdir ~/my-gemstone-project
cd ~/my-gemstone-project

# Download the script and make it executable
curl -fsSLO https://raw.githubusercontent.com/ramauers/gs-cli/HEAD/gs-cli && chmod +x gs-cli
```

### Running the Installer

Execute the script to start the guided interactive setup:

```bash
./gs-cli
```

The script will detect the empty directory and guide you through the configuration. You will be prompted to:
1. **Select a product version:** Press `Enter` to use the recommended default version (**3.7.6**).
2. **Name your stone:** Provide a custom name or press `Enter` for the default `gs64stone`.
3. **Set a NetLDI port:** Specify a port or press `Enter` for the default `50377`.

## Directory Structure

Once the installation is complete, the script downloads and unzips the GemStone/S software, initializing a clean environment with the following layout inside your project directory:

```text
├── conf/                 # Database configuration files
├── fullBackups/          # Target directory for database backups
├── gemstone/             # The unzipped GemStone/S product binaries
│   └── locks/            # Process and system lock files
└── var/                  # Dynamic runtime data
    ├── extent/           # The repository extent files (.dbf)
    ├── log/              # GemStone application and stone logs
    └── tranlog/          # Transaction log files
```

## Usage

Once GemStone/S is installed, executing `./gs-cli --help` will display the built-in help overview with all available management subcommands:

```text
./gs-cli --help
Usage: gs-cli <subcommand> [options]
Subcommands:
    bash                   Run a bash shell with exported environment
    backup                 Make a full backup
    commitRestore          Run SystemRepository commitRestore
    gsStatistics           Print some basic Gemstone/S statistics
    mfc                    Run SystemRepository markForCollection
    reclaimAll             Run SystemRepository reclaimAll
    restoreBackup <file>   Restore from a given full backup file
    revertToGoldenCopy     Drop your Image and revert to golden extent copy
    script <.gs-file>      Execute the given Topaz-Script
    showBackups            List all available backup files
    start                  Start GemStone/S
    status                 Status of GemStone/S
    stop                   Stop GemStone/S
    topaz                  Start a linked topaz session
```


# myPowerShells
my PowerShell scripts



# ## USAGE
mkcd name_of_directory — Create a directory, enter it, open in VS Code

```powershell
function mkcd {
    param (
        [string]$folderName
    )

    # If no folder name is provided — show a hint and exit
    if (-not $folderName) {
        Write-Host "Enter folder name: mkcd MyFolder"
        return
    }

    # If the folder already exists — inform the user and exit
    if (Test-Path $folderName) {
        Write-Host "Folder '$folderName' already exist!"
        return
    }

    # Create the new folder
    mkdir $folderName

    # Change location to the new folder
    Set-Location $folderName

    # Open the current directory in VS Code
    code .
}
```



# UA -FolderName name_of_directory — Find latest ZIP in Downloads, extract all files to C:\Users\Kiril\name_of_direcory, open in VS Code

```powershell
function UA {
    param(
        [Parameter(Mandatory = $true)]
        [string]$FolderName
    )

    # Paths: where ZIPs are located and where to extract them
    $downloads   = "C:\Users\Kiril\Downloads"
    $targetRoot  = "C:\Users\Kiril\zips"
    $targetPath  = Join-Path $targetRoot $FolderName

    # Create the target directory if it doesn't exist
    if (!(Test-Path $targetPath)) {
        New-Item -ItemType Directory -Path $targetPath | Out-Null
    }

    # Find the most recently modified ZIP file in Downloads
    $latestZip = Get-ChildItem -Path $downloads -Filter *.zip |
                 Sort-Object LastWriteTime -Descending |
                 Select-Object -First 1

    # If no ZIP file is found — display a message and exit
    if (-not $latestZip) {
        Write-Host "ZIP not found in $downloads"
        return
    }
```

    # Extract the ZIP archive into the target folder
    Expand-Archive -Path $latestZip.FullName -DestinationPath $targetPath -Force

    # Open the extracted folder in VS Code
    code $targetPath
}

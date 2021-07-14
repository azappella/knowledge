# powershell profile

## Microsoft.PowerShell_profile.ps1

Create a file under `$env:USERPROFILE\Documents\PowerShell\Microsoft.PowerShell_profile.ps1`

```
Import-Module posh-git

## Set the ssh-agent service to manual
## Set-Service ssh-agent -StartupType Manual

function Test-Administrator {
  $user = [Security.Principal.WindowsIdentity]::GetCurrent();
  (New-Object Security.Principal.WindowsPrincipal $user).IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)
}

function Start-Ssh-Agent {
  if ((Get-Service ssh-agent).Status -ne "Running") {
    ssh-agent
  }

  if ((ssh-add -l) -eq "The agent has no identities.") {
    ssh-add $env:USERPROFILE\.ssh\id_ed25519
  }

  # see https://github.com/dahlbyk/posh-git/issues/640
  git config --global core.sshCommand C:/WINDOWS/System32/OpenSSH/ssh.exe

}

function Git-Status { 
  git status 
}

# Use different username if elevated
if (Test-Administrator) {  
  Write-Host "(Elevated) " -NoNewline -ForegroundColor White
}


Start-Ssh-Agent

Set-Alias -Name gs -Value Git-Status

Set-Alias subl "C:\Program Files\Sublime Text 3\sublime_text.exe"
```
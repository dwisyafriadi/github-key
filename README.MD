# 1. Generate SSH Key (SUDAH ✅)
```
ssh-keygen -t ed25519 -C "github-actions-deploy" -f "$env:USERPROFILE\.ssh\github_deploy"
```

# 2. Update SSH Config untuk Port 443 (LAKUKAN INI)
```
@"
Host github.com
    HostName ssh.github.com
    Port 443
    User git
    IdentityFile ~/.ssh/github_deploy
    StrictHostKeyChecking no
"@ | Out-File -FilePath ~/.ssh/config -Encoding ASCII
```

# 3. Tampilkan Public Key (COPY INI KE GITHUB)
```
Get-Content ~/.ssh/github_deploy.pub
```

# Test SSH connection
```
ssh -T git@github.com
```

# Harusnya muncul:
# Hi dwisyafriadi! You've successfully authenticated, but GitHub does not provide shell access.


# Generate SSH Key
```
ssh-keygen -t ed25519 -C "github-actions-deploy" -f ~/.ssh/github_deploy
```

# Tekan ENTER 2x untuk passphrase kosong

# Buat SSH config (pakai port 443 kalau port 22 diblokir)
```
cat > ~/.ssh/config << 'EOF'
Host github.com
    HostName ssh.github.com
    Port 443
    User git
    IdentityFile ~/.ssh/github_deploy
    StrictHostKeyChecking no
EOF
```

# Set permissions yang benar
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/github_deploy
chmod 644 ~/.ssh/github_deploy.pub
chmod 600 ~/.ssh/config
```


# Tampilkan public key (copy ini ke GitHub)
```
cat ~/.ssh/github_deploy.pub
```

# Test port 22 dulu
ssh -T git@github.com

# Kalau timeout, test port 443
ssh -T -p 443 git@ssh.github.com

# Kalau port 443 berhasil, update config pakai port 443 (lihat step 2)

# Ganti remote ke SSH
git remote set-url origin git@github.com:username/repo-name.git

# Verifikasi
git remote -v

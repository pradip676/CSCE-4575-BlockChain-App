# Homework 1 — TypeScript + React Hooks (with WSL on Windows)

This document captures the entire process of setting up my environment and completing **HW-1** step by step.  

---

## 1. Setting up WSL2 on Windows

1. **Enable required Windows features**  
   Open **PowerShell as Administrator** and run:

   ```powershell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
Then reboot your computer.

Check WSL status

wsl --status


You should see Default Version: 2 and virtualization enabled.

Install Ubuntu 22.04

wsl --install -d Ubuntu-22.04


The first launch will prompt you to create a Linux username/password.

2. Install Node.js and pnpm inside WSL

Open the Ubuntu terminal in WSL and run:

# Update packages
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git

# Install Node.js 20.x
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install pnpm globally
sudo npm install -g pnpm


Verify:

node -v
pnpm -v

3. Create the React + TypeScript project

⚠️ Important: Always work in your Linux home folder, not /mnt/c/..., to avoid EPERM errors.

cd ~
mkdir -p projects && cd projects
pnpm create create-react-app hw-1 --template typescript
cd hw-1
pnpm start


Open http://localhost:3000
 — you should see the default CRA page.

4. Replace App.tsx with the homework solution

Open hw-1/src/App.tsx and replace its contents with:
import React, { useRef, useState, useEffect } from 'react';
import './App.css';
// import logo from './logo.svg';

function App() {
  const myRef = useRef<string[]>(
    'Lorem ipsum dolor sit amet consectetur adipiscing elit sed do'.split(' ')
  );

  const [num, setNum] = useState<number | null>(null);

  useEffect(() => {
    setNum(2);
  }, []);

  const onChangeHandler = (evt: React.ChangeEvent<HTMLInputElement>) => {
    const v = evt.target.value;

    // Allow '' and '-' so negatives can be typed for screenshots
    if (v === '' || v === '-') {
      setNum(null);
      return;
    }

    const n = Number(v);
    setNum(Number.isNaN(n) ? 0 : n);
  };

  const words = myRef.current;

  const safeCount =
    typeof num === 'number'
      ? Math.max(0, Math.min(Math.floor(num), words.length))
      : 0;

  const list = words.slice(0, safeCount);

  return (
    <div className="App">
      <header className="App-header">
        <ul>
          {list.map((w, i) => (
            <li key={i}>{w}</li>
          ))}
        </ul>

        <input
          type="number"
          value={num ?? ''}
          onChange={onChangeHandler}
        />
      </header>
    </div>
  );
}

export default App;

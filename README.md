name: Build Android APK

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build_apk:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Repository
      uses: actions/checkout@v3

    - name: Install Dependencies
      run: |
        sudo apt update
        sudo apt install -y python3-pip build-essential python3-dev git \
        libsdl2-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-ttf-dev \
        libportmidi-dev libswscale-dev libavformat-dev libavcodec-dev zlib1g-dev
        pip install --upgrade pip
        pip install buildozer kivy

    - name: Build APK
      run: |
        buildozer android debug

    - name: Upload APK
      uses: actions/upload-artifact@v3
      with:
        name: offlinechat-apk
        path: bin/*.apk

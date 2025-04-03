FROM ubuntu:20.04

# Устанавливаем зависимости
RUN apt-get update && apt-get install -y \
    openjdk-11-jdk \
    wget \
    unzip \
    libqt5widgets5 \
    libgl1-mesa-glx \
    && rm -rf /var/lib/apt/lists/*

# Устанавливаем Android SDK
ENV ANDROID_HOME=/opt/android-sdk
RUN mkdir -p $ANDROID_HOME
WORKDIR $ANDROID_HOME

# Загружаем Android SDK
RUN wget -q https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip \
    && unzip commandlinetools-linux-9477386_latest.zip -d cmdline-tools \
    && mv cmdline-tools cmdline-tools/latest \
    && rm commandlinetools-linux-9477386_latest.zip

# Устанавливаем необходимые пакеты SDK
ENV PATH=$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/emulator:$PATH
RUN yes | sdkmanager --licenses \
    && sdkmanager "platform-tools" "platforms;android-34" "system-images;android-34;google_apis;x86_64" "emulator"

RUN echo "no" | avdmanager create avd -n test_emulator -k "system-images;android-34;google_apis;x86_64" --device "pixel_6"

CMD emulator -avd test_emulator -no-audio -no-window -gpu swiftshader_indirect -verbose


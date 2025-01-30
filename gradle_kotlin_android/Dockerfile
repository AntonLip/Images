FROM gradle:8.4.0-jdk17-alpine AS builder

# Установка необходимых пакетов
RUN apk add --no-cache \
    wget unzip

# Задаем версии Android SDK и Command Line Tools через ARG
ARG ANDROID_SDK_VERSION=34
ARG CMDLINE_TOOLS_VERSION=11076708

# Установка Android SDK
ENV ANDROID_SDK_ROOT=/opt/android-sdk
RUN mkdir -p $ANDROID_SDK_ROOT/cmdline-tools && \
    wget -q https://dl.google.com/android/repository/commandlinetools-linux-${CMDLINE_TOOLS_VERSION}_latest.zip -O /tmp/cmdline-tools.zip && \
    unzip /tmp/cmdline-tools.zip -d $ANDROID_SDK_ROOT/cmdline-tools && \
    mv $ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools $ANDROID_SDK_ROOT/cmdline-tools/latest && \
    rm /tmp/cmdline-tools.zip

# Установка нужных инструментов SDK
ENV PATH=$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$PATH
RUN yes | sdkmanager --licenses && \
    sdkmanager "platform-tools" "platforms;android-${ANDROID_SDK_VERSION}" "build-tools;${ANDROID_SDK_VERSION}.0.0"

# Установка прав на Gradle и Android SDK
RUN chmod -R 777 /opt/android-sdk

# Устанавливаем рабочую директорию
WORKDIR /app

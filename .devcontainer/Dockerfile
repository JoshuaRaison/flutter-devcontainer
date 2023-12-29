# Use the Java development container as base
FROM mcr.microsoft.com/devcontainers/java:1-17-bookworm

# Set Debian frontend to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# Update and install necessary packages
RUN apt-get update && apt-get install -y git wget unzip

# Switch to 'developer' user before cloning and configuring Flutter
USER vscode

# Set the working directory to /sdk
WORKDIR /sdk

# Download and set up Android command line tools
RUN mkdir -p /sdk/android/cmdline-tools && \
    wget -q https://dl.google.com/android/repository/commandlinetools-linux-10406996_latest.zip -O cmdline-tools.zip && \
    unzip -q cmdline-tools.zip -d /sdk/android/cmdline-tools && \
    mv /sdk/android/cmdline-tools/cmdline-tools /sdk/android/cmdline-tools/latest && \
    rm cmdline-tools.zip

# Set environment variables
ENV ANDROID_HOME=/sdk/android

# Add Android SDK tools to the PATH
ENV PATH="${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/platform-tools:${PATH}"

# Install Android SDK tools
RUN yes | sdkmanager "platform-tools" "platforms;android-33" "build-tools;33.0.1"

# Clone Flutter SDK and configure Git for safe directory
RUN git clone https://github.com/flutter/flutter.git -b stable --depth 1 /sdk/flutter && \
    git -C /sdk/flutter config --global --add safe.directory /sdk/flutter

# Add Flutter to the PATH
ENV PATH="$PATH:/sdk/flutter/bin"

# Disable analytics and configure Flutter
RUN flutter doctor && \
    flutter --disable-analytics && \
    flutter config --android-sdk "$ANDROID_HOME" && \
    yes | flutter doctor --android-licenses

# Set the final working directory
WORKDIR /workspaces

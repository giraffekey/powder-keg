.PHONY: xcodebuild run install boot-sim generate clean

DEVICE = ${DEVICE_ID}
ifndef DEVICE_ID
	DEVICE=$(shell xcrun simctl list devices 'iOS' | grep -v 'unavailable' | grep -v '^--' | grep -v '==' | head -n 1 | grep -E -o -i "([0-9a-f]{8}-([0-9a-f]{4}-){3}[0-9a-f]{12})")
endif

run: install
	xcrun simctl launch --console $(DEVICE) com.masterofgiraffe.powderkeg

boot-sim:
	xcrun simctl boot $(DEVICE) || true

install: xcodebuild-simulator boot-sim
	xcrun simctl install $(DEVICE) build/Build/Products/Debug-iphonesimulator/mobile.app

xcodebuild-simulator:
	IOS_TARGETS=x86_64-apple-ios xcodebuild -scheme mobile -configuration Debug -derivedDataPath build -destination "id=$(DEVICE)"

xcodebuild-iphone:
	IOS_TARGETS=aarch64-apple-ios xcodebuild -scheme mobile -configuration Debug -derivedDataPath build -arch arm64

xcodebuild-iphone-release:
	IOS_TARGETS=aarch64-apple-ios xcodebuild -scheme mobile -configuration Release -derivedDataPath build -arch arm64

clean:
	rm -r build
	cargo clean

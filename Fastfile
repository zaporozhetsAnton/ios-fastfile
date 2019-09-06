# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
#     https://docs.fastlane.tools/plugins/available-plugins
#

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

default_platform(:ios)

platform :ios do

    desc "Sync of certificates and provision profiles"
        lane :certificates do
            match(type: "development")
            match(type: "adhoc")
            match(type: "appstore")
    end

    desc "Re-generate the provisioning profile"
        lane :updateCertificates do
            match(type: "development", force: true)
            match(type: "adhoc", force: true)
            match(type: "appstore", force: true)
    end

    desc "Register new device"
        lane :register_new_device do |options|
            device_name = prompt(text: "Enter the device name: ")
            device_udid = prompt(text: "Enter the device UDID: ")
            device_hash = {}
            device_hash[device_name] = device_udid
            register_devices(
                devices: device_hash
            )
            updateCertificates
    end

    desc "Create build"
        lane :build do
            match(type: "adhoc")
            build_app(
                scheme: "Farmgate",
                silent: true,
                output_directory: "~/Desktop/FarmGateBuild",
                configuration: "Release",
                export_method: "ad-hoc",
                clean: true
             )
    end

    desc "Create build and upload it to AppStore"
        lane :release do
            match(type: "appstore")
            build_app(
                scheme: "Farmgate",
                silent: true,
                export_method: "app-store",
                clean: true,
                configuration: "Distribution",
                output_directory: "build"
            )
            upload_to_testflight(
                skip_submission: true,
                skip_waiting_for_build_processing: true,
                ipa: "build/FarmGate.ipa"
            )
    end

end

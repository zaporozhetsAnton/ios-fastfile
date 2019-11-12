# Change IPA_NAME in upload_to_testflight function to the name of your project
# Change PROJECT_SCHEME in build_app function to the scheme name of your project

default_platform(:ios)

platform :ios do
    
    before_all do
        if is_ci?
            begin
                delete_keychain
            rescue => ex
                UI.error(ex)
            end
            create_keychain(default_keychain: true, unlock: true, timeout: 3600)
        end
    end

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
                scheme: "PROJECT_SCHEME",
                silent: true,
                output_directory: "build",
                configuration: "Release",
                export_method: "ad-hoc",
                clean: true
             )
    end

    desc "Create build and upload it to AppStore"
        lane :release do
            match(type: "appstore")
            build_app(
                scheme: "PROJECT_SCHEME",
                silent: true,
                export_method: "app-store",
                clean: true,
                configuration: "Distribution",
                output_directory: "build"
            )
            upload_to_testflight(
                skip_submission: true,
                skip_waiting_for_build_processing: true,
                ipa: "build/IPA_NAME.ipa"
            )
    end

end

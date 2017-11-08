# Customise this file, documentation can be found here:
# https://github.com/fastlane/fastlane/tree/master/fastlane/docs
# All available actions: https://docs.fastlane.tools/actions
# can also be listed using the `fastlane actions` command

# Change the syntax highlighting to Ruby
# All lines starting with a # are ignored when running `fastlane`

# If you want to automatically update fastlane if a new version is available:
# update_fastlane

# This is the minimum version number required.
# Update this, if you use features of a newer version
fastlane_version "2.28.4"

default_platform :android

platform :android do

  desc "Build, sign, align only"
  lane :bsa do |options|
    gradle(task: "clean")
    gradle(task: "assembleRelease")
    Dir.glob(File.join("../app/build/outputs/apk/**", "*iTest*.apk")).each {|fname| File.delete(fname)}
    sign_apk(
      apk_path: File.expand_path(Dir.glob(File.join("../app/build/outputs/apk/**", "*.apk"))[0]),
      keystore_path: ENV["HOME"] + "/Documents/the-path-to-your-own.keystore",
      alias: "your-own-private-key-name",
      keypass: options[:keypass],
      storepass: options[:storepass]
    )
    zipalign(apk_path: lane_context[SharedValues::SIGNED_APK_PATH])
  end

  desc "Deploy a new version to Google Play Alpha"
  lane :alpha do |options|
    gradle(task: "clean")
    gradle(task: "assembleRelease")
    Dir.glob(File.join("../app/build/outputs/apk/**", "*iTest*.apk")).each {|fname| File.delete(fname)}
    sign_apk(
      apk_path: File.expand_path(Dir.glob(File.join("../app/build/outputs/apk/**", "*.apk"))[0]),
      keystore_path: ENV["HOME"] + "/Documents/the-path-to-your-own.keystore",
      alias: "your-own-private-key-name",
      keypass: options[:keypass],
      storepass: options[:storepass]
    )
    zipalign(apk_path: lane_context[SharedValues::SIGNED_APK_PATH])
    supply(
      track: "alpha",
      apk: lane_context[SharedValues::ALIGNED_APK_PATH],
      skip_upload_metadata: true,
      skip_upload_images: true,
      skip_upload_screenshots: true
    )
  end

end


# More information about multiple platforms in fastlane: https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Platforms.md
# All available actions: https://docs.fastlane.tools/actions

# fastlane reports which actions are used
# No personal data is sent or shared. Learn more at https://github.com/fastlane/enhancer

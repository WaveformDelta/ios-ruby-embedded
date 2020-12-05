XCODEROOT = %x[xcode-select -print-path].strip
SIMSDKPATH = Dir["#{XCODEROOT}/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator*.sdk/"].sort.last
IOSSDKPATH = Dir["#{XCODEROOT}/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS*.sdk/"].sort.last

task :verify_sysroot => [SIMSDKPATH, IOSSDKPATH]

file "ios_build_config.rb" do

  open('custom.gembox', 'w') do |gembox_file|
    gembox_file.puts <<__EOF__
MRuby::GemBox.new do |conf|
  # Use standard Kernel#sprintf method
  conf.gem :core => "mruby-sprintf"

  # Use standard print/puts/p
  conf.gem :core => "mruby-print"

  # Use standard Math module
  conf.gem :core => "mruby-math"

  # Use standard Time class
  conf.gem :core => "mruby-time"

  # Use standard Struct class
  conf.gem :core => "mruby-struct"

  # Use extensional Enumerable module
  conf.gem :core => "mruby-enum-ext"

  # Use extensional String class
  conf.gem :core => "mruby-string-ext"

  # Use extensional Numeric class
  conf.gem :core => "mruby-numeric-ext"

  # Use extensional Array class
  conf.gem :core => "mruby-array-ext"

  # Use extensional Hash class
  conf.gem :core => "mruby-hash-ext"

  # Use extensional Range class
  conf.gem :core => "mruby-range-ext"

  # Use extensional Proc class
  conf.gem :core => "mruby-proc-ext"

  # Use extensional Symbol class
  conf.gem :core => "mruby-symbol-ext"

  # Use Random class
  conf.gem :core => "mruby-random"

  # Use extensional Object class
  conf.gem :core => "mruby-object-ext"

  # Use ObjectSpace class
  conf.gem :core => "mruby-objectspace"

  # Use Fiber class
  conf.gem :core => "mruby-fiber"

  # Use Enumerator class (require mruby-fiber)
  conf.gem :core => "mruby-enumerator"

  # Use Enumerable::Lazy class (require mruby-enumerator)
  conf.gem :core => "mruby-enum-lazy"

  # Use extended toplevel object (main) methods
  conf.gem :core => "mruby-toplevel-ext"

  # Use extensional Kernel module
  conf.gem :core => "mruby-kernel-ext"
  
  # WvD ADDITION: Use extensional Class module
  conf.gem :core => "mruby-class-ext"
  
  # WvD ADDITION: Use error module
  conf.gem :core => "mruby-error"
  
  # WvD ADDITION: Use eval module
  conf.gem :core => "mruby-eval"
  
  # WvD ADDITION: Use errno module
  conf.gem :mgem => "mruby-errno"
  
  # WvD ADDITION: Use IO module
  conf.gem :mgem => "mruby-io"
  
  # WvD ADDITION: Use pack module
  conf.gem :github => 'iij/mruby-pack'
  
  # WvD ADDITION: Use sleep module
  conf.gem :github => 'matsumotory/mruby-sleep'
  
  # WvD ADDITION: Use the Onigmo regex module
  conf.gem :mgem => 'mruby-onig-regexp'
  
  # WvD ADDITION: Use marshal module
  conf.gem :mgem => 'mruby-marshal'
 
end
__EOF__
  end

  FileUtils.cp "custom.gembox", "mruby/mrbgems/"

  open('ios_build_config.rb', 'w') do |config_file|

    config_file.puts <<__EOF__
MRuby::Build.new do |conf|
  toolchain :clang

  conf.cc.defines = %w(MRB_INTEGER_DIVISION)
  conf.cxx.defines = %w(MRB_INTEGER_DIVISION)
  
  conf.gembox 'default'

  # WvD ADDITION: Use error module
  conf.gem :core => "mruby-error"
  
  # WvD ADDITION: Use eval module
  conf.gem :core => "mruby-eval"
  
  # WvD ADDITION: Use errno module
  conf.gem :mgem => "mruby-errno"
  
  # WvD ADDITION: Use IO module
  conf.gem :mgem => "mruby-io"
  
  # WvD ADDITION: Use pack module
  conf.gem :github => 'iij/mruby-pack'
  
  # WvD ADDITION: Use sleep module
  conf.gem :github => 'matsumotory/mruby-sleep'
  
  # WvD ADDITION: use the Onigmo regex module
  conf.gem :mgem => 'mruby-onig-regexp'

  # WvD ADDITION: Use marshal module
  conf.gem :mgem => 'mruby-marshal'
end

SIM_SYSROOT="#{SIMSDKPATH}"
DEVICE_SYSROOT="#{IOSSDKPATH}"

MRuby::CrossBuild.new('i386-apple-darwin') do |conf|
  conf.bins = []

  conf.gembox 'custom'

  conf.cc do |cc|
    cc.command = 'xcrun'
    cc.defines = %w(MRB_INTEGER_DIVISION)
    cc.flags = %W(-sdk iphoneos clang -miphoneos-version-min=8.0 -arch i386 -isysroot \#{SIM_SYSROOT} -g -O3 -Wall -fexceptions -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.cxx do |cxx|
    cxx.command = 'xcrun'
    cxx.defines = %w(MRB_INTEGER_DIVISION)
    cxx.flags = %W(-sdk iphoneos clang -x c++ -miphoneos-version-min=8.0 -arch i386 -isysroot \#{SIM_SYSROOT} -g -O3 -Wall -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.linker do |linker|
    linker.command = 'xcrun'
    linker.flags = %W(-sdk iphoneos clang -x c++ -miphoneos-version-min=8.0 -arch i386 -isysroot \#{SIM_SYSROOT})
  end
end

MRuby::CrossBuild.new('x86_64-apple-darwin') do |conf|
  conf.bins = []

  conf.gembox 'custom'

  conf.cc do |cc|
    cc.command = 'xcrun'
    cc.defines = %w(MRB_INTEGER_DIVISION)
    cc.flags = %W(-sdk iphoneos clang -miphoneos-version-min=8.0 -arch x86_64 -isysroot \#{SIM_SYSROOT} -g -O3 -Wall -fexceptions -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.cxx do |cxx|
    cxx.command = 'xcrun'
    cxx.defines = %w(MRB_INTEGER_DIVISION)
    cxx.flags = %W(-sdk iphoneos clang -x c++ -miphoneos-version-min=8.0 -arch x86_64 -isysroot \#{SIM_SYSROOT} -g -O3 -Wall -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.linker do |linker|
    linker.command = 'xcrun'
    linker.flags = %W(-sdk iphoneos clang -x c++ -miphoneos-version-min=8.0 -arch x86_64 -isysroot \#{SIM_SYSROOT})
  end
end

MRuby::CrossBuild.new('armv7-apple-darwin') do |conf|
  conf.bins = []

  conf.gembox 'custom'

  conf.cc do |cc|
    cc.command = 'xcrun'
    cc.defines = %w(MRB_INTEGER_DIVISION)
    cc.flags = %W(-sdk iphoneos clang -arch armv7 -isysroot \#{DEVICE_SYSROOT} -g -O3 -Wall -fexceptions -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.cxx do |cxx|
    cxx.command = 'xcrun'
    cxx.defines = %w(MRB_INTEGER_DIVISION)
    cxx.flags = %W(-sdk iphoneos clang -x c++ -arch armv7 -isysroot \#{DEVICE_SYSROOT} -g -O3 -Wall -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.linker do |linker|
    linker.command = 'xcrun'
    linker.flags = %W(-sdk iphoneos clang -x c++ -arch armv7 -isysroot \#{DEVICE_SYSROOT})
  end
end

MRuby::CrossBuild.new('armv7s-apple-darwin') do |conf|
  conf.bins = []

  conf.gembox 'custom'

  conf.cc do |cc|
    cc.command = 'xcrun'
    cc.defines = %w(MRB_INTEGER_DIVISION)
    cc.flags = %W(-sdk iphoneos clang -arch armv7s -isysroot \#{DEVICE_SYSROOT} -g -O3 -Wall -fexceptions -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.cxx do |cxx|
    cxx.command = 'xcrun'
    cxx.defines = %w(MRB_INTEGER_DIVISION)
    cxx.flags = %W(-sdk iphoneos clang -x c++ -arch armv7s -isysroot \#{DEVICE_SYSROOT} -g -O3 -Wall -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.linker do |linker|
    linker.command = 'xcrun'
    linker.flags = %W(-sdk iphoneos clang -x c++ -arch armv7s -isysroot \#{DEVICE_SYSROOT})
  end
end

MRuby::CrossBuild.new('aarch64-apple-darwin') do |conf|
  conf.bins = []

  conf.gembox 'custom'

  conf.cc do |cc|
    cc.command = 'xcrun'
    cc.defines = %w(MRB_INTEGER_DIVISION)
    cc.flags = %W(-sdk iphoneos clang -arch arm64 -isysroot \#{DEVICE_SYSROOT} -g -O3 -Wall -fexceptions -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.cxx do |cxx|
    cxx.command = 'xcrun'
    cxx.defines = %w(MRB_INTEGER_DIVISION)
    cxx.flags = %W(-sdk iphoneos clang -x c++ -arch arm64 -isysroot \#{DEVICE_SYSROOT} -g -O3 -Wall -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.linker do |linker|
    linker.command = 'xcrun'
    linker.flags = %W(-sdk iphoneos clang -x c++ -arch arm64 -isysroot \#{DEVICE_SYSROOT})
  end
end

MRuby::CrossBuild.new('arm-apple-darwin') do |conf|
  conf.bins = []

  conf.gembox 'custom'

  conf.cc do |cc|
    cc.command = 'xcrun'
    cc.defines = %w(MRB_INTEGER_DIVISION)
    cc.flags = %W(-sdk iphoneos clang -arch arm64e -isysroot \#{DEVICE_SYSROOT} -g -O3 -Wall -fexceptions -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.cxx do |cxx|
    cxx.command = 'xcrun'
    cxx.defines = %w(MRB_INTEGER_DIVISION)
    cxx.flags = %W(-sdk iphoneos clang -x c++ -arch arm64e -isysroot \#{DEVICE_SYSROOT} -g -O3 -Wall -fembed-bitcode -Werror-implicit-function-declaration)
  end

  conf.linker do |linker|
    linker.command = 'xcrun'
    linker.flags = %W(-sdk iphoneos clang -x c++ -arch arm64e -isysroot \#{DEVICE_SYSROOT})
  end
end
__EOF__

  end

end

directory "MRuby.framework/Versions"
directory "MRuby.framework/Versions/1.0.0/"
directory "MRuby.framework/Versions/1.0.0/Headers"
directory "MRuby.framework/Versions/1.0.0/Resources"

task "MRuby.framework/Versions/1.0.0/" => "MRuby.framework/Versions" do

  Dir.chdir("MRuby.framework/Versions/") do
    File.symlink("1.0.0", "Current")
  end
  Dir.chdir("MRuby.framework/") do
    File.symlink("Versions/Current/Headers", "Headers")
    File.symlink("Versions/Current/MRuby", "MRuby")
    File.symlink("Versions/Current/Resources", "Resources")
  end
end

task :build_mruby => "ios_build_config.rb" do
  Dir.chdir("mruby") do
    ENV['MRUBY_CONFIG'] = "../ios_build_config.rb"
    sh "rake"
  end
end

directory "bin"

file "bin/mirb" => [:build_mruby, "bin"] do
  FileUtils.cp "mruby/build/host/bin/mirb", "bin/mirb"
end

file "bin/mrbc" => [:build_mruby, "bin"] do
  FileUtils.cp "mruby/build/host/bin/mrbc", "bin/mrbc"
end

file "bin/mruby" => [:build_mruby, "bin"] do
  FileUtils.cp "mruby/build/host/bin/mruby", "bin/mruby"
end

file "MRuby.framework/Versions/Current/MRuby" => [:build_mruby, "MRuby.framework/Versions/1.0.0/"] do
  sh "#{XCODEROOT}/Toolchains/XcodeDefault.xctoolchain/usr/bin/lipo -arch i386 mruby/build/i386-apple-darwin/lib/libmruby.a"\
  " -arch x86_64 mruby/build/x86_64-apple-darwin/lib/libmruby.a"\
  " -arch arm64e mruby/build/arm-apple-darwin/lib/libmruby.a"\
  " -arch arm64 mruby/build/aarch64-apple-darwin/lib/libmruby.a"\
  " -arch armv7 mruby/build/armv7-apple-darwin/lib/libmruby.a"\
  " -arch armv7s mruby/build/armv7s-apple-darwin/lib/libmruby.a"\
  " -create -output MRuby.framework/Versions/Current/MRuby"
end

task :mruby_headers => [:build_mruby, "MRuby.framework/Versions/1.0.0/Headers"] do
  FileUtils.cp_r "mruby/include/.", "MRuby.framework/Versions/Current/Headers/"
end
 
task :all => [:verify_sysroot, "bin/mirb", "bin/mrbc", "bin/mruby", "MRuby.framework/Versions/Current/MRuby", :mruby_headers]

task :default => :all

task :clean => "ios_build_config.rb" do
  Dir.chdir("mruby") do
    ENV['MRUBY_CONFIG'] = "../ios_build_config.rb"
    sh "rake clean"
  end
  FileUtils.rm_f ["ios_build_config.rb", "bin/mirb", "bin/mrbc", "bin/mruby", "custom.gembox", "mruby/mrbgems/custom.gembox"]
  FileUtils.rm_rf "MRuby.framework"
end

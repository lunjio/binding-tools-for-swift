SWIFT_BIN = ../../bin/swift/bin
SWIFT_LIB = ../../bin/swift/lib/swift/macosx/


SWIFTC = $(SWIFT_BIN)/swiftc
SDK = `xcrun --sdk macosx --show-sdk-path`
SWIFTARGS = -v -sdk $(SDK) -L $(SWIFT_LIB) -emit-module -emit-library

BINDINGTOOLSFORSWIFT=../../binding-tools-for-swift

OUTPUT_MODULE=Sandwiches

libSandwiches.dylib: *.swift 
	@rm -rf XamWrappingSource armv7 armv7s arm64 x86_64
	@mkdir XamGlue.framework
	@cp ../../lib/SwiftInterop/mac/XamGlue.framework/XamGlue .
	@cp ../../lib/binding-tools-for-swift/SwiftRuntimeLibrary.dll .
	$(SWIFTC) $(SWIFTARGS) -module-name $(OUTPUT_MODULE) *.swift
	$(BINDINGTOOLSFORSWIFT) --retain-swift-wrappers -o . -C . -module-name $(OUTPUT_MODULE) 
	@xcrun install_name_tool -change "@rpath/XamGlue.framework/XamGlue" XamGlue libXamWrapping.dylib
	mcs -nowarn:CS0169 -lib:../lib -r:SwiftRuntimeLibrary -lib:. *.cs -unsafe+ -out:$(OUTPUT_MODULE).exe

runit:
	@DYLD_LIBRARY_PATH=$(SWIFT_LIB):$(SWIFT_GLUE):. mono --arch=64 $(OUTPUT_MODULE).exe

clean:
	@rm -f *.dylib *.swiftmodule *.swiftdoc *.dll *.exe TopLevelEntities.cs IBreadSandwiches.cs IFillingSandwiches.cs HamSandwiches.cs RyeSandwiches.cs XamGlue
	@rm -rf XamWrappingSource armv7 armv7s arm64 x86_64 bindings

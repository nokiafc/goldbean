#The apk for your currently selected variant (app 
goldbeanapply plugin: 'com.android.application'
apply plugin: 'kotlin-android-extensions'
apply plugin: 'kotlin-android'
if (!isDebug()) {
    apply plugin: 'newlens'
}
def config = rootProject.ext.ver
def library = rootProject.ext.library

def gitSha() {
    try {
        String gitRev = 'git rev-parse --short HEAD'.execute(null, project.rootDir).text.trim()
        if (gitRev == null) {
            throw new GradleException("can't get git rev, you should add git to system path or just input test value, such as 'testTinkerId'")
        }
        return gitRev
    } catch (Exception e) {
        throw new GradleException("can't get git rev, you should add git to system path or just input test value, such as 'testTinkerId'")
    }
}


def javaVersion = JavaVersion.VERSION_1_8

android {
    compileSdkVersion 27
    defaultConfig {
        minSdkVersion config.minSdkVersion
        targetSdkVersion config.targetSdkVersion
        versionCode config.versionCode
        versionName config.versionName
        multiDexEnabled true
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
        multiDexKeepProguard file("multidexKeep.pro")
        ndk {
            abiFilters 'armeabi-v7a', 'x86'
        }

        /**
         * buildConfig can change during patch!
         * we can use the newly value when patch
         */
        buildConfigField "String", "MESSAGE", "\"I am the base apk\""
//        buildConfigField "String", "MESSAGE", "\"I am the patch apk\""
        /**
         * client version would update with patch
         * so we can get the newly git version easily!
         */
        buildConfigField "String", "TINKER_ID", "\"${getTinkerIdValue()}\""
        buildConfigField "String", "PLATFORM", "\"all\""
    }

    signingConfigs {
        cpt {
            storeFile file(rootDir.toString() + '\\LotteryBible.jks')
            storePassword 'ruanjie'
            keyAlias 'caipiao'
            keyPassword 'ruanjie'
        }
        llb{
            storeFile file(rootDir.toString() + '\\linglingba.jks')
            storePassword '111111'
            keyAlias 'rdc'
            keyPassword '111111'
        }
//        xgcp{
//            storeFile file(rootDir.toString() + '\\xgcp.jks')
//            storePassword '123456'
//            keyAlias 'rdc'
//            keyPassword '123456'
//        }
    }

    flavorDimensions "brand", "version"

    productFlavors {
        xgcp {
            dimension "brand"
            applicationId "com.rdc.xgcp"
            buildConfigField "String", "APP_TYPE", "\"goucai\""//应用类型
            buildConfigField "String", "UMENG_KEY", "\"5c63fe57f1f55623560006b3\""//友盟
            buildConfigField "String", "HUANXIN_KEY", "\"1429190213068099#kefuchannelapp65763\""//环信
            buildConfigField "String", "HUANXIN_TENANT_ID", "\"95915\""//环信
            buildConfigField "String", "BUGLY_ID", "\"b107115301\""//bugly
            //qq
            buildConfigField "String", "QQ_APPID", "\"101548151\""
            buildConfigField "String", "QQ_APPKEY", "\"98764aba20a7dcf4f770e54c76109b91\""
            //微信
            buildConfigField "String", "WX_APPID", "\"wx4fafbce3d5676594\""
            buildConfigField "String", "WX_APPKEY", "\"569548c23dbc847d6034a1052f29a992\""

            manifestPlaceholders = [
                    qqappid      : "101548151",
                    app_icon     : "@mipmap/ic_launcher",
                    app_name     : "香港彩",
                    JPUSH_PKGNAME: "com.rdc.xgcp",//极光资讯版
                    JPUSH_APPKEY : "1521ed902a6721078bbb7dcc",
                    JPUSH_CHANNEL: "developer-default", //暂时填写默认值即可.
            ]
            if (isDebug()) {
                manifestPlaceholders["app_name"] = "香港彩资讯测试版"
            }
        }
        llb {
            dimension "brand"
            applicationId "com.rdc.linglingba.zixun"
            buildConfigField "String", "APP_TYPE", "\"zixun\""//应用类型
            buildConfigField "String", "UMENG_KEY", "\"5c3dacc9b465f504af0006de\""//友盟
            buildConfigField "String", "HUANXIN_KEY", "\"1429190115068344#kefuchannelapp64187\""//环信
            buildConfigField "String", "HUANXIN_TENANT_ID", "\"93929\""//环信
            buildConfigField "String", "BUGLY_ID", "\"18fc05a01d\""//bugly
            //qq
            buildConfigField "String", "QQ_APPID", "\"101542711\""
            buildConfigField "String", "QQ_APPKEY", "\"8b2e23de0c2ed957dfd0c44a01ad3a7\""
            //微信
            buildConfigField "String", "WX_APPID", "\"wx0f0f1bcf56586cc5\""
            buildConfigField "String", "WX_APPKEY", "\"da8924b48d2d7209d1c608dc02c5e5db\""
            signingConfig signingConfigs.llb

            manifestPlaceholders = [
                    qqappid      : "101542711",
                    app_icon     : "@mipmap/ic_launcher",
                    app_name     : "008资讯版",
                    JPUSH_PKGNAME: "com.rdc.linglingba.zixun",//极光资讯版
                    JPUSH_APPKEY : "ad05908dc9ca789035e97070",
                    JPUSH_CHANNEL: "developer-default", //暂时填写默认值即可.
            ]
            if (isDebug()) {
                manifestPlaceholders["app_name"] = "008资讯测试版"
                manifestPlaceholders["JPUSH_APPKEY"] = "68022ebb9ee34e1930721be8"
            }
        }
        cpt {
            dimension "brand"
            applicationId "com.ruanjie.lotterybible"
            buildConfigField "String", "APP_TYPE", "\"zixun\""//应用类型
            buildConfigField "String", "UMENG_KEY", "\"5be44c26f1f556a70100031f\""//友盟
            buildConfigField "String", "HUANXIN_KEY", "\"1113180915228439#cpbd\""//环信
            buildConfigField "String", "HUANXIN_TENANT_ID", "\"60342\""//环信
            buildConfigField "String", "BUGLY_ID", "\"fa71a0e262\""//bugly
            //qq
            buildConfigField "String", "QQ_APPID", "\"101517011\""
            buildConfigField "String", "QQ_APPKEY", "\"8b78d6d792872fff1b525b62505e747d\""
            //微信
            buildConfigField "String", "WX_APPID", "\"wxb5985d7a813de973\""
            buildConfigField "String", "WX_APPKEY", "\"c66e86edc1e1403e1a48b86c1dabc779\""
            signingConfig signingConfigs.cpt

            manifestPlaceholders = [
                    qqappid      : "101517011",
                    app_icon     : "@mipmap/ic_launcher",
                    app_name     : "CPT",
                    JPUSH_PKGNAME: "com.ruanjie.lotterybible",            //极光资讯版
                    JPUSH_APPKEY : "bdc914029a58caaacaf37e4a",
                    JPUSH_CHANNEL: "developer-default", //暂时填写默认值即可.
            ]
            if (isDebug()) {
                manifestPlaceholders["app_name"] = "cpt测试版"
                manifestPlaceholders["JPUSH_APPKEY"] = "14387b88be5ecf48c1a2d47b"
            }
        }
        zixun {
            dimension "version"
            buildConfigField "String", "APP_TYPE", "\"zixun\""//应用类型
        }
        goucai {
            dimension "version"
            buildConfigField "String", "APP_TYPE", "\"goucai\""//应用类型
        }
    }

    variantFilter { variant ->
        def names = variant.flavors*.name
        // To check for a certain build type, use variant.buildType.name == "<buildType>"
        if (names.contains("cpt") && names.contains("zixun")
                || names.contains("llb") && names.contains("goucai")
                || names.contains("xgcp") && names.contains("zixun")) {
            // Gradle ignores any variants that satisfy the conditions above.
            setIgnore(true)
        }
    }


    buildTypes {
        debug {
            multiDexKeepProguard file('tinker_multidexkeep.pro')
            minifyEnabled true
            shrinkResources false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            useProguard true
            ext.enableCrashlytics = false
            signingConfig release.signingConfig
        }
        release {
            multiDexKeepProguard file('tinker_multidexkeep.pro')
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            useProguard true
        }
    }

    compileOptions {
        targetCompatibility javaVersion
        sourceCompatibility javaVersion
    }

    lintOptions {
        checkReleaseBuilds false
        abortOnError false
    }
    dexOptions {
        jumboMode = true
        javaMaxHeapSize "8g"
    }

    sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
    }

    //跳过Lint和Test相关的task, 以加速编译
    if (isDebug()) {
        gradle.taskGraph.whenReady {
            tasks.each { task ->
                if (task.name.contains("Test") || task.name.contains("Lint")) {
                    task.enabled = false
                }
            }
        }
    }

}

repositories {

    flatDir {
        dirs 'libs'
    }
    mavenCentral()
}




dependencies {
    implementation fileTree(include: ['*.jar'], dir: 'libs')
//    implementation fileTree(include: ['*.aar'], dir: 'libs')
    implementation 'com.android.support:appcompat-v7:27.1.1'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
    annotationProcessor library.butterknife_compiler
    annotationProcessor library.glide_compiler
    implementation "com.github.bumptech.glide:okhttp3-integration:4.6.1"
    implementation library.multidex
    implementation library.okio
    implementation library.okhttp
    implementation library.okhttp_logging
    implementation library.gson
    implementation library.retrofit2
    implementation library.retrofit2_gson
    implementation library.retrofit2_adapter
    implementation library.TabLayout


    implementation library.PickerView
    implementation library.androideventbus
    implementation 'com.github.florent37:arclayout:1.0.3'
    implementation 'com.ogaclejapan.arclayout:library:1.1.0@aar'
    implementation 'com.bigkoo:ConvenientBanner:2.1.4'
    implementation 'com.sunfusheng:marqueeview:1.3.3'
    implementation 'com.oushangfeng:PinnedSectionItemDecoration:1.2.7'
    //    implementation 'com.github.PhilJay:MPAndroidChart:v3.1.0-alpha'
    implementation project(':MPChartLib')
    implementation project(':kefu-easeui')
    implementation project(':baselibrary')
    implementation 'com.google.android:flexbox:1.0.0'
    implementation 'com.google.guava:guava:26.0-android'
    implementation 'com.github.chrisbanes:PhotoView:2.1.4'
    implementation 'com.google.zxing:core:3.3.3'
    implementation 'com.jaeger.ninegridimageview:library:1.1.1'
    implementation 'com.ycjiang:ImagePreview:2.2.2'
    implementation 'pub.devrel:easypermissions:1.0.1'
    implementation 'cn.bingoogolapple:bga-photopicker:1.2.8@aar'
    implementation 'cn.bingoogolapple:bga-baseadapter:1.2.7@aar'
    implementation 'com.umeng.sdk:share-core:6.9.2'
    implementation 'com.umeng.sdk:share-qq:6.9.1.1'
    implementation 'com.umeng.sdk:share-wechat:6.9.2'
    implementation 'com.umeng.sdk:common:1.5.1'
    implementation 'me.jessyan:autosize:1.0.6'
    implementation 'com.github.instacart.truetime-android:library-extension-rx:3.4'
    implementation 'jp.wasabeef:glide-transformations:4.0.1'
    implementation 'jp.wasabeef:richeditor-android:1.2.2'
    implementation 'com.github.crosswall:Android-Coverflow:release-v1.0.5'
    implementation 'com.allenliu.versionchecklib:library:2.1.6'
    implementation 'com.dyhdyh.loadingbar:loadingbar:1.4.7'
    //bugly
    implementation 'com.tencent.bugly:crashreport:2.8.6.0'
    implementation 'com.tencent.bugly:nativecrashreport:3.6.0.1'

    //可选，用于生成application类
    annotationProcessor("com.tencent.tinker:tinker-android-anno:${TINKER_VERSION}")
    compileOnly("com.tencent.tinker:tinker-android-anno:${TINKER_VERSION}")
    //tinker的核心库
    implementation("com.tencent.tinker:tinker-android-lib:${TINKER_VERSION}")

    //极光推送
    implementation 'cn.jiguang.sdk:jpush:3.1.6'
    implementation 'cn.jiguang.sdk:jcore:1.2.5'

    //听云
    implementation "com.networkbench.newlens.agent.android:nbs.newlens.agent:2.11.0"
    implementation 'com.networkbench.newlens.agent.android:nbs.newlens.nativecrash:1.0.0'

    //luban压缩
    implementation 'top.zibin:Luban:1.1.8'
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    // Optional, if you use support library fragments:
    debugImplementation 'com.squareup.leakcanary:leakcanary-support-fragment:1.6.3'
}

//tinker-------start
def bakPath = file("${buildDir}/bakApk/")

/**
 * you can use assembleRelease to build you base apk
 * use tinkerPatchRelease -POLD_APK=  -PAPPLY_MAPPING=  -PAPPLY_RESOURCE= to build patch
 * add apk from the build/bakApk
 */
ext {
    //for some reason, you may want to ignore tinkerBuild, such as instant run debug build?
    tinkerEnabled = false

    //for normal build
    //old apk file to build patch apk
    tinkerOldApkPath = ""
    //proguard mapping file to build patch apk
    tinkerApplyMappingPath = ""
    //resource R.txt to build patch apk, must input if there is resource changed
    tinkerApplyResourcePath = ""

    //only use for build all flavor, if not, just ignore this field
    tinkerBuildFlavorDirectory = "${bakPath}/app-1123-01-25-55"
}


def getOldApkPath() {
    return hasProperty("OLD_APK") ? OLD_APK : ext.tinkerOldApkPath
}

def getApplyMappingPath() {
    return hasProperty("APPLY_MAPPING") ? APPLY_MAPPING : ext.tinkerApplyMappingPath
}

def getApplyResourceMappingPath() {
    return hasProperty("APPLY_RESOURCE") ? APPLY_RESOURCE : ext.tinkerApplyResourcePath
}

def getTinkerIdValue() {
    return hasProperty("TINKER_ID") ? TINKER_ID : gitSha()
}

def buildWithTinker() {
    return hasProperty("TINKER_ENABLE") ? Boolean.parseBoolean(TINKER_ENABLE) : ext.tinkerEnabled
}

def getTinkerBuildFlavorDirectory() {
    return ext.tinkerBuildFlavorDirectory
}

if (buildWithTinker()) {
    apply plugin: 'com.tencent.tinker.patch'

    tinkerPatch {
        /**
         * necessary，default 'null'
         * the old apk path, use to diff with the icon_new apk to build
         * add apk from the build/bakApk
         */
        oldApk = getOldApkPath()
        /**
         * optional，default 'false'
         * there are some cases we may get some warnings
         * if ignoreWarning is true, we would just assert the patch process
         * case 1: minSdkVersion is below 14, but you are using dexMode with raw.
         *         it must be crash when load.
         * case 2: newly added Android Component in AndroidManifest.xml,
         *         it must be crash when load.
         * case 3: loader classes in dex.loader{} are not keep in the main dex,
         *         it must be let tinker not work.
         * case 4: loader classes in dex.loader{} changes,
         *         loader classes is ues to load patch dex. it is useless to change them.
         *         it won't crash, but these changes can't effect. you may ignore it
         * case 5: resources.arsc has changed, but we don't use applyResourceMapping to build
         */
        ignoreWarning = false

        /**
         * optional，default 'true'
         * whether sign the patch file
         * if not, you must do yourself. otherwise it can't check success during the patch loading
         * we will use the sign config with your build type
         */
        useSign = true

        /**
         * optional，default 'true'
         * whether use tinker to build
         */
        tinkerEnable = buildWithTinker()

        /**
         * Warning, applyMapping will affect the normal android build!
         */
        buildConfig {
            /**
             * optional，default 'null'
             * if we use tinkerPatch to build the patch apk, you'd better to apply the old
             * apk mapping file if minifyEnabled is enable!
             * Warning:
             * you must be careful that it will affect the normal assemble build!
             */
            applyMapping = getApplyMappingPath()
            /**
             * optional，default 'null'
             * It is nice to keep the resource id from R.txt file to reduce java changes
             */
            applyResourceMapping = getApplyResourceMappingPath()

            /**
             * necessary，default 'null'
             * because we don't want to check the base apk with md5 in the runtime(it is slow)
             * tinkerId is use to identify the unique base apk when the patch is tried to apply.
             * we can use git rev, svn rev or simply versionCode.
             * we will gen the tinkerId in your manifest automatic
             */
            tinkerId = getTinkerIdValue()

            /**
             * if keepDexApply is true, class in which dex refer to the old apk.
             * open this can reduce the dex diff file size.
             */
            keepDexApply = false

            /**
             * optional, default 'false'
             * Whether tinker should treat the base apk as the one being protected by app
             * protection tools.
             * If this attribute is true, the generated patch package will contain a
             * dex including all changed classes instead of any dexdiff patch-info files.
             */
            isProtectedApp = false

            /**
             * optional, default 'false'
             * Whether tinker should support component hotplug (add icon_new component dynamically).
             * If this attribute is true, the component added in icon_new apk will be available after
             * patch is successfully loaded. Otherwise an error would be announced when generating patch
             * on compile-time.
             *
             * <b>Notice that currently this feature is incubating and only support NON-EXPORTED Activity</b>
             */
            supportHotplugComponent = false
        }

        dex {
            /**
             * optional，default 'jar'
             * only can be 'raw' or 'jar'. for raw, we would keep its original format
             * for jar, we would repack dexes with zip format.
             * if you want to support below 14, you must use jar
             * or you want to save rom or check quicker, you can use raw mode also
             */
            dexMode = "jar"

            /**
             * necessary，default '[]'
             * what dexes in apk are expected to deal with tinkerPatch
             * it support * or ? pattern.
             */
            pattern = ["classes*.dex",
                       "assets/secondary-dex-?.jar"]
            /**
             * necessary，default '[]'
             * Warning, it is very very important, loader classes can't change with patch.
             * thus, they will be removed from patch dexes.
             * you must put the following class into main dex.
             * Simply, you should add your own application {@code tinker.sample.android.SampleApplication}
             * own tinkerLoader, and the classes you use in them
             *
             */
            loader = [
                    //use sample, let BaseBuildInfo unchangeable with tinker
                    "tinker.sample.android.app.BaseBuildInfo"
            ]
        }

        lib {
            /**
             * optional，default '[]'
             * what library in apk are expected to deal with tinkerPatch
             * it support * or ? pattern.
             * for library in assets, we would just recover them in the patch directory
             * you can get them in TinkerLoadResult with Tinker
             */
            pattern = ["lib/*/*.so"]
        }

        res {
            /**
             * optional，default '[]'
             * what resource in apk are expected to deal with tinkerPatch
             * it support * or ? pattern.
             * you must include all your resources in apk here,
             * otherwise, they won't repack in the icon_new apk resources.
             */
            pattern = ["res/*", "assets/*", "resources.arsc", "AndroidManifest.xml"]

            /**
             * optional，default '[]'
             * the resource file exclude patterns, ignore add, delete or modify resource change
             * it support * or ? pattern.
             * Warning, we can only use for files no relative with resources.arsc
             */
            ignoreChange = ["assets/sample_meta.txt"]

            /**
             * default 100kb
             * for modify resource, if it is larger than 'largeModSize'
             * we would like to use bsdiff algorithm to reduce patch file size
             */
            largeModSize = 100
        }

        packageConfig {
            /**
             * optional，default 'TINKER_ID, TINKER_ID_VALUE' 'NEW_TINKER_ID, NEW_TINKER_ID_VALUE'
             * package meta file gen. path is assets/package_meta.txt in patch file
             * you can use securityCheck.getPackageProperties() in your ownPackageCheck method
             * or TinkerLoadResult.getPackageConfigByName
             * we will get the TINKER_ID from the old apk manifest for you automatic,
             * other config files (such as patchMessage below)is not necessary
             */
            configField("patchMessage", "tinker is sample to use")
            /**
             * just a sample case, you can use such as sdkVersion, brand, channel...
             * you can parse it in the SamplePatchListener.
             * Then you can use patch conditional!
             */
            configField("platform", "all")
            /**
             * patch version via packageConfig
             */
            configField("patchVersion", "1.0")
        }
        //or you can add config filed outside, or get meta value from old apk
        //project.tinkerPatch.packageConfig.configField("test1", project.tinkerPatch.packageConfig.getMetaDataFromOldApk("Test"))
        //project.tinkerPatch.packageConfig.configField("test2", "sample")

        /**
         * if you don't use zipArtifact or path, we just use 7za to try
         */
        sevenZip {
            /**
             * optional，default '7za'
             * the 7zip artifact path, it will use the right 7za with your platform
             */
            zipArtifact = "com.tencent.mm:SevenZip:1.1.10"
            /**
             * optional，default '7za'
             * you can specify the 7za path yourself, it will overwrite the zipArtifact value
             */
//        path = "/usr/local/bin/7za"
        }
    }

    List<String> flavors = new ArrayList<>()
    project.android.productFlavors.each { flavor ->
        flavors.add(flavor.name)
    }
    boolean hasFlavors = flavors.size() > 0
    def date = new Date().format("MMdd-HH-mm-ss")

    /**
     * bak apk and mapping
     */
    android.applicationVariants.all { variant ->
        /**
         * task type, you want to bak
         */
        def taskName = variant.name

        tasks.all {
            if ("assemble${taskName.capitalize()}".equalsIgnoreCase(it.name)) {

                it.doLast {
                    copy {
                        def fileNamePrefix = "${project.name}-${variant.baseName}"
                        def newFileNamePrefix = hasFlavors ? "${fileNamePrefix}" : "${fileNamePrefix}-${date}"

                        def destPath = hasFlavors ? file("${bakPath}/${project.name}-${date}/${variant.flavorName}") : bakPath
                        from variant.outputs.first().outputFile
                        into destPath
                        rename { String fileName ->
                            fileName.replace("${fileNamePrefix}.apk", "${newFileNamePrefix}.apk")
                        }

                        from "${buildDir}/outputs/mapping/${variant.dirName}/mapping.txt"
                        into destPath
                        rename { String fileName ->
                            fileName.replace("mapping.txt", "${newFileNamePrefix}-mapping.txt")
                        }

                        from "${buildDir}/intermediates/symbols/${variant.dirName}/R.txt"
                        into destPath
                        rename { String fileName ->
                            fileName.replace("R.txt", "${newFileNamePrefix}-R.txt")
                        }
                    }
                }
            }
        }
    }
    project.afterEvaluate {
        //sample use for build all flavor for one time
        if (hasFlavors) {
            task(tinkerPatchAllFlavorRelease) {
                group = 'tinker'
                def originOldPath = getTinkerBuildFlavorDirectory()
                for (String flavor : flavors) {
                    def tinkerTask = tasks.getByName("tinkerPatch${flavor.capitalize()}Release")
                    dependsOn tinkerTask
                    def preAssembleTask = tasks.getByName("process${flavor.capitalize()}ReleaseManifest")
                    preAssembleTask.doFirst {
                        String flavorName = preAssembleTask.name.substring(7, 8).toLowerCase() + preAssembleTask.name.substring(8, preAssembleTask.name.length() - 15)
                        project.tinkerPatch.oldApk = "${originOldPath}/${flavorName}/${project.name}-${flavorName}-release.apk"
                        project.tinkerPatch.buildConfig.applyMapping = "${originOldPath}/${flavorName}/${project.name}-${flavorName}-release-mapping.txt"
                        project.tinkerPatch.buildConfig.applyResourceMapping = "${originOldPath}/${flavorName}/${project.name}-${flavorName}-release-R.txt"

                    }

                }
            }

            task(tinkerPatchAllFlavorDebug) {
                group = 'tinker'
                def originOldPath = getTinkerBuildFlavorDirectory()
                for (String flavor : flavors) {
                    def tinkerTask = tasks.getByName("tinkerPatch${flavor.capitalize()}Debug")
                    dependsOn tinkerTask
                    def preAssembleTask = tasks.getByName("process${flavor.capitalize()}DebugManifest")
                    preAssembleTask.doFirst {
                        String flavorName = preAssembleTask.name.substring(7, 8).toLowerCase() + preAssembleTask.name.substring(8, preAssembleTask.name.length() - 13)
                        project.tinkerPatch.oldApk = "${originOldPath}/${flavorName}/${project.name}-${flavorName}-debug.apk"
                        project.tinkerPatch.buildConfig.applyMapping = "${originOldPath}/${flavorName}/${project.name}-${flavorName}-debug-mapping.txt"
                        project.tinkerPatch.buildConfig.applyResourceMapping = "${originOldPath}/${flavorName}/${project.name}-${flavorName}-debug-R.txt"
                    }

                }
            }
        }
    }
}



task sortPublicTxt() {
    doLast {
        File originalFile = project.file("public.txt")
        File sortedFile = project.file("public_sort.txt")
        List<String> sortedLines = new ArrayList<>()
        originalFile.eachLine {
            sortedLines.add(it)
        }
        Collections.sort(sortedLines)
        sortedFile.delete()
        sortedLines.each {
            sortedFile.append("${it}\n")
        }
    }
}
//tinker-------end

---------------------
Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())

ext {

    ver = [
            //Version
            versionCode                  : 26,
            versionName                  : "2.0.1",

            //SDK And Tools
            minSdkVersion                : 17,
            targetSdkVersion             : 25,
            compileSdkVersion            : 27,

            //Dependencies
            supportLibraryVersion        : '27.1.1',
            multidex                     : '1.0.2',

            //libraryVersion
            log                          : '2.2.0',//无混淆
            rxjava2                      : '2.1.14',
            rxandroid                    : '2.0.2',
            rxlifecycle2                 : '2.2.1',
            SmartRefreshLayout           : '1.0.5.1',//无需混淆
            BaseRecyclerViewAdapterHelper: '2.9.42',
            butterknife                  : '8.8.1',
            retrofit2                    : '2.4.0',
            okio                         : '1.14.0',
            okhttp                       : '3.10.0',
            gson                         : '2.8.2',
            glide                        : '4.8.0',

    ]

    library = [
            multidex                     : "com.android.support:multidex:$ver.multidex",
            design                       : "com.android.support:design:$ver.supportLibraryVersion",
            cardview                     : "com.android.support:cardview-v7:$ver.supportLibraryVersion",
            supportV4                    : "com.android.support:support-v4:$ver.supportLibraryVersion",
            supportV7                    : "com.android.support:appcompat-v7:$ver.supportLibraryVersion",
            recyclerview                 : "com.android.support:recyclerview-v7:$ver.supportLibraryVersion",
            constraint                   : 'com.android.support.constraint:constraint-layout:1.0.2',

            logger                       : "com.orhanobut:logger:$ver.log",
            rxjava2                      : "io.reactivex.rxjava2:rxjava:$ver.rxjava2",
            rxandroid                    : "io.reactivex.rxjava2:rxandroid:$ver.rxandroid",
            rxpermissions2               : "com.tbruyelle.rxpermissions2:rxpermissions:0.9.5@aar",
            rxlifecycle2_components      : "com.trello.rxlifecycle2:rxlifecycle-components:$ver.rxlifecycle2",
            SmartRefreshLayout           : "com.scwang.smartrefresh:SmartRefreshLayout:$ver.SmartRefreshLayout",
            BaseRecyclerViewAdapterHelper: "com.github.CymChad:BaseRecyclerViewAdapterHelper:$ver.BaseRecyclerViewAdapterHelper",

            butterknife                  : "com.jakewharton:butterknife:$ver.butterknife",
            butterknife_compiler         : "com.jakewharton:butterknife-compiler:$ver.butterknife",

            okio                         : "com.squareup.okio:okio:$ver.okio",
            okhttp                       : "com.squareup.okhttp3:okhttp:$ver.okhttp",
            okhttp_logging               : "com.squareup.okhttp3:logging-interceptor:$ver.okhttp",
            retrofit2                    : "com.squareup.retrofit2:retrofit:$ver.retrofit2",
            retrofit2_gson               : "com.squareup.retrofit2:converter-gson:$ver.retrofit2",
            retrofit2_adapter            : "com.squareup.retrofit2:adapter-rxjava2:$ver.retrofit2",
            gson                         : "com.google.code.gson:gson:$ver.gson",
            glide                        : "com.github.bumptech.glide:glide:$ver.glide",
            glide_compiler               : "com.github.bumptech.glide:compiler:$ver.glide",
            indexablestickylistview      : "me.yokeyword:indexablestickylistview:0.6.10",
            androideventbus              : "org.simple:androideventbus:1.0.5.1",
            ezviz                        : "com.hikvision.ezviz:ezviz-sdk:4.7.1",
            jpush                        : "cn.jiguang.sdk:jpush:3.0.9",
            jcore                        : "cn.jiguang.sdk:jcore:1.1.7",
            circleimageview              : 'de.hdodenhof:circleimageview:2.2.0',
            TabLayout                    : 'com.flyco.tablayout:FlycoTabLayout_Lib:2.1.2@aar',
            //高德地图
            amap                         : 'com.amap.api:location:latest.integration',
            ShortcutBadger               : 'me.leolin:ShortcutBadger:1.1.19@aar',
            PickerView                   : 'com.contrarywind:Android-PickerView:3.2.5',
            BaseToolbar                  : 'com.mirkowu:BaseToolbar:0.0.5',
            StatusBarUtil                : 'com.mirkowu:StatusBarUtil:0.0.2',
    ]

    //keystore
    keystorePath = rootDir.toString() + '\\LotteryBible.jks'

    keystorePassword = 'ruanjie'
    alias = 'caipiao'
    aliasPassword = 'ruanjie'
}

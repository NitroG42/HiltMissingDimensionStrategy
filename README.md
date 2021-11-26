# Hilt and MissingDimensionStrategy

## Issue Tracker
https://issuetracker.google.com/issues/207785213

## Context

The project contains 2 modules, one library module declaring two flavors (free and paid), and one app module that uses the library module.
As the App module doesn't declare the two flavors, we want it to use by default the free flavor. For that we declare the following in the
app's build.gradle :

```
    missingDimensionStrategy 'app', 'free'
```

## Issue

If the app module use the Hilt gradle plugin, we get the following error while compiling the project :

```
Could not determine the dependencies of task ':app:hiltJavaCompileDebug'.
> Could not resolve all task dependencies for configuration ':app:hiltCompileOnlyDebug'.
   > Could not resolve project :mylibrary.
     Required by:
         project :app
      > The consumer was configured to find a runtime of a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug'. However we cannot choose between the following variants of project :mylibrary:
          - freeDebugRuntimeElements
          - paidDebugRuntimeElements
        All of them match the consumer attributes:
          - Variant 'freeDebugRuntimeElements' capability HiltMissingDimensionStrategy:mylibrary:unspecified declares a runtime of a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug':
              - Unmatched attributes:
                  - Provides attribute 'com.android.build.api.attributes.AgpVersionAttr' with value '7.1.0-beta03' but the consumer didn't ask for it
                  - Provides attribute 'com.android.build.api.attributes.ProductFlavor:app' with value 'free' but the consumer didn't ask for it
                  - Provides attribute 'com.android.build.gradle.internal.attributes.VariantAttr' with value 'freeDebug' but the consumer didn't ask for it
                  - Provides a library but the consumer didn't ask for it
                  - Provides attribute 'org.gradle.jvm.environment' with value 'android' but the consumer didn't ask for it
                  - Provides attribute 'org.jetbrains.kotlin.platform.type' with value 'androidJvm' but the consumer didn't ask for it
          - Variant 'paidDebugRuntimeElements' capability HiltMissingDimensionStrategy:mylibrary:unspecified declares a runtime of a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug':
              - Unmatched attributes:
                  - Provides attribute 'com.android.build.api.attributes.AgpVersionAttr' with value '7.1.0-beta03' but the consumer didn't ask for it
                  - Provides attribute 'com.android.build.api.attributes.ProductFlavor:app' with value 'paid' but the consumer didn't ask for it
                  - Provides attribute 'com.android.build.gradle.internal.attributes.VariantAttr' with value 'paidDebug' but the consumer didn't ask for it
                  - Provides a library but the consumer didn't ask for it
                  - Provides attribute 'org.gradle.jvm.environment' with value 'android' but the consumer didn't ask for it
                  - Provides attribute 'org.jetbrains.kotlin.platform.type' with value 'androidJvm' but the consumer didn't ask for it
        The following variants were also considered but didn't match the requested attributes:
          - Variant 'freeDebugApiElements' capability HiltMissingDimensionStrategy:mylibrary:unspecified declares a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug':
              - Incompatible because this component declares an API of a component and the consumer needed a runtime of a component
          - Variant 'freeReleaseApiElements' capability HiltMissingDimensionStrategy:mylibrary:unspecified:
              - Incompatible because this component declares an API of a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'release' and the consumer needed a runtime of a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug'
          - Variant 'freeReleaseRuntimeElements' capability HiltMissingDimensionStrategy:mylibrary:unspecified declares a runtime of a component:
              - Incompatible because this component declares a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'release' and the consumer needed a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug'
          - Variant 'paidDebugApiElements' capability HiltMissingDimensionStrategy:mylibrary:unspecified declares a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug':
              - Incompatible because this component declares an API of a component and the consumer needed a runtime of a component
          - Variant 'paidReleaseApiElements' capability HiltMissingDimensionStrategy:mylibrary:unspecified:
              - Incompatible because this component declares an API of a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'release' and the consumer needed a runtime of a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug'
          - Variant 'paidReleaseRuntimeElements' capability HiltMissingDimensionStrategy:mylibrary:unspecified declares a runtime of a component:
              - Incompatible because this component declares a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'release' and the consumer needed a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug'

```

If we comment the line in app build.gradle :

```
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
    id 'kotlin-kapt'
    //id 'dagger.hilt.android.plugin'
}
```

The project compile.


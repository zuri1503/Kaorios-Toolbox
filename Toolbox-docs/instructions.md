# Kaorios Toolbox Guide

## Step 1: Add to `system/build.prop`

```properties
persist.sys.kaorios=kousei
```

---

## Step 2: Import `classes.dex`

Import **classes.dex** to the **last classes** of `framework.jar`.

---

## Step 3: Modify `framework.jar` classes

### Class: `ApplicationPackageManager`

#### Method: `hasSystemFeature(Ljava/lang/String;)Z`

Find:
```smali
return v0
.end method
```

Add **above**:
```smali
invoke-static {v0, p1}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosAttestationBL(ZLjava/lang/String;)Z
move-result v0
```

---

#### Method: `hasSystemFeature(Ljava/lang/String;I)Z`

Find:
```smali
return v0
.end method
```

Add **above**:
```smali
invoke-static {p1, p2, v0}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosFeatures(Ljava/lang/String;IZ)Z
move-result v0
```

---

### Class: `Instrumentation`

#### Method: `newApplication(Ljava/lang/Class;Landroid/content/Context;)Landroid/app/Application;`

Find:
```smali
return-object v0
.end method
```

Add **above**:
```smali
invoke-static {p1}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosProps(Landroid/content/Context;)V
```

---

#### Method: `newApplication(Ljava/lang/ClassLoader;Ljava/lang/String;Landroid/content/Context;)Landroid/app/Application;`

Find:
```smali
return-object v0
.end method
```

Add **above**:
```smali
invoke-static {p3}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosProps(Landroid/content/Context;)V
```

---

### Class: `KeyStore2`

#### Method: `getKeyEntry(Landroid/system/keystore2/KeyDescriptor;)Landroid/system/keystore2/KeyEntryResponse;`

Find:
```smali
return-object v0
.end method
```

Add **above**:
```smali
invoke-static {v0}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosKeybox(Landroid/system/keystore2/KeyEntryResponse;)Landroid/system/keystore2/KeyEntryResponse;
move-result-object v0
```

---

### Class: `AndroidKeyStoreSpi`

#### Method: `engineGetCertificateChain(Ljava/lang/String;)[Ljava/security/cert/Certificate;`

Add **below** `.registers XX`:
```smali
invoke-static {}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosPropsEngineGetCertificateChain()V
```

---

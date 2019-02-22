# RPC 服务支持参数类型说明

## 特殊说明

1. 定义微服务方法的参数类型为 `long` 时，数字长度超过14位之后，使用者需要字符串传输，例如：`"1234567890123456748"`，建议开发者定义接口的时候，定义为 `String` 类型，然后再解析为 `long`，且添加参数说明。
1. 定义微服务方法的参数类型为 `double`，而且小数点后超过8位时，使用者使用的时候需要字符串传输，例如：`"1.1234567891211"`，建议开发者定义接口的时候定义为 `String` 类型，再解析为 `double`，且者添加参数说明。
1. 定义微服务方法的参数类型为 `boolean`，参数值为 `false` 时，使用者使用时需要字符串传输，例如：`"false"`,建议开发者定义接口的时候定义为 `String` 类型，再解析为 `boolean`, 且添加参数说明。
1. 定义微服方法的参数类型实体，实体有字符串类型的属性 `str`，返回类型也为实体，而且实体也有字符串类型的数据 `str`，使用者发送的 `str="null"`,返回的实体里面，`str="\"null"\"` 会多带双引号,如果开发者定义接口的时候，如果有影响，建议在返回参数里面加以说明，或者避开 `null`。
1. 定义微服务方法的参数类型为实体时，实体不支持 `byte[]` 类型的属性。
1. 定义微服务返回参数类型为 `java.lang.Character []` 时，网关返回的是由 `java.lang.Character` 构成的字符串，需要使用者再处理一下才能得到 `java.lang.Character []`,建议开发者定义接口的时候，返回类型用json对象包装一下，且添加返回参数说明。
1. 定义微服务返回参数为实体，且实体有数组类型的属性时，网关暂时支持 `null` 或者 `[object,object,…]`,暂时不支该属性以 `[]` 空数组传递，建议开发者定义接口的时候，这类属性添加说明备注一下。

## 8 个 Java 基本数据类型

|类型|验证是否通过|备注|
|:-:|-|:-|
|int|通过||
|double|通过|由于精度影响，与字面输入值有差异|
|short|通过||
|char|通过||
|long|通过||
|byte|通过||
|float|通过|由于精度影响，与字面输入值有差异|
|boolean|通过||

## 8 个 Java 基本数据类型构成的数组

|类型|验证是否通过|备注|
|:-:|-|:-|
|int[]|通过||	
|double[]|通过|由于精度影响，与字面输入值有差异|
|short[]|通过||	
|char[]|未通过|不支持，方案解决，用 `java.lang.Character` 数组代替|
|long[]|通过||
|byte[]|通过||
|float[]|通过|由于精度影响，与字面输入值有差异|
|boolean[]|通过||

## 9 个 java 包装类

|类型|验证是否通过|备注|
|:-|-|:-|
|java.lang.Integer|通过||	
|java.lang.Double|通过|由于精度影响，与字面输入值有差异|
|java.lang.Short|通过||
|java.lang.Character|通过||
|java.lang.Long|通过|长度大于14需要字符串形式传输："123456789012345612"; 小于14正常传输：12345678901234；|
|java.lang.Byte|通过||
|java.lang.Float|通过|由于精度影响，与字面输入值有差异|
|java.lang.Boolean|通过||
|Java.lang.String|通过||


## 9 个 java 包装类构成的数组

|类型|验证是否通过|备注|
|:-|-|:-|
|java.lang.Integer[]|通过||
|java.lang.Double[]|通过|由于精度影响，与字面输入值有差异||
|java.lang.Short[]|通过||
|java.lang.Character[]|通过|`["d","1"]`|
|java.lang.Long[]|通过||
|java.lang.Byte[]|通过||
|java.lang.Float[]|通过|由于精度影响，与字面输入值有差异|
|java.lang.Boolean[]|通过||
|java.lang.String[]|通过||

## 由 8 个 java 基本数据类型、9 个 java 包装类以及其数组所构成的复杂实体

**验证是否通过：** 通过

**备注：** `byte[]` 数组域不支持

**实体demo：**

```java
package com.yonyou.cloud.ms.entity;

public class SimpleEntity {

    private boolean booleanField;
    private byte byteField;
    private char charField;
    private short shortField;
    private int intField;
    private long longField;
    private float floatField;
    private double doubleField;

    private Boolean fieldBoolean;
    private Byte fieldByte;
    private Character fieldCharacter;
    private Short fieldShort;
    private Integer fieldInteger;
    private Long fieldLong;
    private Float fieldFloat;
    private Double fieldDouble;

    private String fieldString;

    private boolean[] booleanArrayField;
//    private byte[] byteArrayField; // 不支持
    private char[] charArrayField;
    private short[] shortArrayField;
    private int[] intArrayField;
    private long[] longArrayField;
    private float[] floatArrayField;
    private double[] doubleArrayField;

    private Boolean[] fieldBooleanArray;
    private Byte[] fieldByteArray;
    private Character[] fieldCharacterArray;
    private Short[] fieldShortArray;
    private Integer[] fieldIntegerArray;
    private Long[] fieldLongArray;
    private Float[] fieldFloatArray;
    private Double[] fieldDoubleArray;

    private String[] fieldStringArray;

    public boolean isBooleanField() {
        return booleanField;
    }

    public void setBooleanField(boolean booleanField) {
        this.booleanField = booleanField;
    }

    public byte getByteField() {
        return byteField;
    }

    public void setByteField(byte byteField) {
        this.byteField = byteField;
    }

    public char getCharField() {
        return charField;
    }

    public void setCharField(char charField) {
        this.charField = charField;
    }

    public short getShortField() {
        return shortField;
    }

    public void setShortField(short shortField) {
        this.shortField = shortField;
    }

    public int getIntField() {
        return intField;
    }

    public void setIntField(int intField) {
        this.intField = intField;
    }

    public long getLongField() {
        return longField;
    }

    public void setLongField(long longField) {
        this.longField = longField;
    }

    public float getFloatField() {
        return floatField;
    }

    public void setFloatField(float floatField) {
        this.floatField = floatField;
    }

    public double getDoubleField() {
        return doubleField;
    }

    public void setDoubleField(double doubleField) {
        this.doubleField = doubleField;
    }

    public Boolean getFieldBoolean() {
        return fieldBoolean;
    }

    public void setFieldBoolean(Boolean fieldBoolean) {
        this.fieldBoolean = fieldBoolean;
    }

    public Byte getFieldByte() {
        return fieldByte;
    }

    public void setFieldByte(Byte fieldByte) {
        this.fieldByte = fieldByte;
    }

    public Character getFieldCharacter() {
        return fieldCharacter;
    }

    public void setFieldCharacter(Character fieldCharacter) {
        this.fieldCharacter = fieldCharacter;
    }

    public Short getFieldShort() {
        return fieldShort;
    }

    public void setFieldShort(Short fieldShort) {
        this.fieldShort = fieldShort;
    }

    public Integer getFieldInteger() {
        return fieldInteger;
    }

    public void setFieldInteger(Integer fieldInteger) {
        this.fieldInteger = fieldInteger;
    }

    public Long getFieldLong() {
        return fieldLong;
    }

    public void setFieldLong(Long fieldLong) {
        this.fieldLong = fieldLong;
    }

    public Float getFieldFloat() {
        return fieldFloat;
    }

    public void setFieldFloat(Float fieldFloat) {
        this.fieldFloat = fieldFloat;
    }

    public Double getFieldDouble() {
        return fieldDouble;
    }

    public void setFieldDouble(Double fieldDouble) {
        this.fieldDouble = fieldDouble;
    }

    public String getFieldString() {
        return fieldString;
    }

    public void setFieldString(String fieldString) {
        this.fieldString = fieldString;
    }

    public boolean[] getBooleanArrayField() {
        return booleanArrayField;
    }

    public void setBooleanArrayField(boolean[] booleanArrayField) {
        this.booleanArrayField = booleanArrayField;
    }

//    public byte[] getByteArrayField() {
//    	return byteArrayField;
//    }
//
//    public void setByteArrayField(byte[] byteArrayField) {
//    	this.byteArrayField = byteArrayField;
//    }

    public char[] getCharArrayField() {
        return charArrayField;
    }

    public void setCharArrayField(char[] charArrayField) {
        this.charArrayField = charArrayField;
    }

    public short[] getShortArrayField() {
        return shortArrayField;
    }

    public void setShortArrayField(short[] shortArrayField) {
        this.shortArrayField = shortArrayField;
    }

    public int[] getIntArrayField() {
        return intArrayField;
    }

    public void setIntArrayField(int[] intArrayField) {
        this.intArrayField = intArrayField;
    }

    public long[] getLongArrayField() {
        return longArrayField;
    }

    public void setLongArrayField(long[] longArrayField) {
        this.longArrayField = longArrayField;
    }

    public float[] getFloatArrayField() {
        return floatArrayField;
    }

    public void setFloatArrayField(float[] floatArrayField) {
        this.floatArrayField = floatArrayField;
    }

    public double[] getDoubleArrayField() {
        return doubleArrayField;
    }

    public void setDoubleArrayField(double[] doubleArrayField) {
        this.doubleArrayField = doubleArrayField;
    }

    public Boolean[] getFieldBooleanArray() {
        return fieldBooleanArray;
    }

    public void setFieldBooleanArray(Boolean[] fieldBooleanArray) {
        this.fieldBooleanArray = fieldBooleanArray;
    }

    public Byte[] getFieldByteArray() {
        return fieldByteArray;
    }

    public void setFieldByteArray(Byte[] fieldByteArray) {
        this.fieldByteArray = fieldByteArray;
    }

    public Character[] getFieldCharacterArray() {
        return fieldCharacterArray;
    }

    public void setFieldCharacterArray(Character[] fieldCharacterArray) {
        this.fieldCharacterArray = fieldCharacterArray;
    }

    public Short[] getFieldShortArray() {
        return fieldShortArray;
    }

    public void setFieldShortArray(Short[] fieldShortArray) {
        this.fieldShortArray = fieldShortArray;
    }

    public Integer[] getFieldIntegerArray() {
        return fieldIntegerArray;
    }

    public void setFieldIntegerArray(Integer[] fieldIntegerArray) {
        this.fieldIntegerArray = fieldIntegerArray;
    }

    public Long[] getFieldLongArray() {
        return fieldLongArray;
    }

    public void setFieldLongArray(Long[] fieldLongArray) {
        this.fieldLongArray = fieldLongArray;
    }

    public Float[] getFieldFloatArray() {
        return fieldFloatArray;
    }

    public void setFieldFloatArray(Float[] fieldFloatArray) {
        this.fieldFloatArray = fieldFloatArray;
    }

    public Double[] getFieldDoubleArray() {
        return fieldDoubleArray;
    }

    public void setFieldDoubleArray(Double[] fieldDoubleArray) {
        this.fieldDoubleArray = fieldDoubleArray;
    }

    public String[] getFieldStringArray() {
        return fieldStringArray;
    }

    public void setFieldStringArray(String[] fieldStringArray) {
        this.fieldStringArray = fieldStringArray;
    }

}
```



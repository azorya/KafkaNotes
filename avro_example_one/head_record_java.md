```java
/**
 * Autogenerated by Avro
 *
 * DO NOT EDIT DIRECTLY
 */
package com.zgconsultants.avro.schema;

import org.apache.avro.specific.SpecificData;

@SuppressWarnings("all")
@org.apache.avro.specific.AvroGenerated
public class HeadRecord extends org.apache.avro.specific.SpecificRecordBase implements org.apache.avro.specific.SpecificRecord {
  private static final long serialVersionUID = 7286028727730814360L;
  public static final org.apache.avro.Schema SCHEMA$ = new org.apache.avro.Schema.Parser().parse("{\"type\":\"record\",\"name\":\"HeadRecord\",\"namespace\":\"com.zgconsultants.avro.schema\",\"fields\":[{\"name\":\"dtF\",\"type\":\"long\",\"doc\":\"myDateTime doc string\"},{\"name\":\"sF\",\"type\":[\"null\",{\"type\":\"string\",\"avro.java.string\":\"String\"}],\"doc\":\"sField could be null\",\"default\":null},{\"name\":\"lF\",\"type\":\"long\",\"doc\":\"lucky number\",\"default\":777},{\"name\":\"bF1\",\"type\":[\"null\",\"boolean\"],\"doc\":\"default could be only null, so is redundant here\",\"default\":null},{\"name\":\"bF2\",\"type\":[\"boolean\",\"null\"],\"doc\":\"default is boolean , so it could be true or false\",\"default\":true}]}");
  public static org.apache.avro.Schema getClassSchema() { return SCHEMA$; }
  /** myDateTime doc string */
   private long dtF;
  /** sField could be null */
   private java.lang.String sF;
  /** lucky number */
   private long lF;
  /** default could be only null, so is redundant here */
   private java.lang.Boolean bF1;
  /** default is boolean , so it could be true or false */
   private java.lang.Boolean bF2;

  /**
   * Default constructor.  Note that this does not initialize fields
   * to their default values from the schema.  If that is desired then
   * one should use <code>newBuilder()</code>.
   */
  public HeadRecord() {}

  /**
   * All-args constructor.
   * @param dtF myDateTime doc string
   * @param sF sField could be null
   * @param lF lucky number
   * @param bF1 default could be only null, so is redundant here
   * @param bF2 default is boolean , so it could be true or false
   */
  public HeadRecord(java.lang.Long dtF, java.lang.String sF, java.lang.Long lF, java.lang.Boolean bF1, java.lang.Boolean bF2) {
    this.dtF = dtF;
    this.sF = sF;
    this.lF = lF;
    this.bF1 = bF1;
    this.bF2 = bF2;
  }

  public org.apache.avro.Schema getSchema() { return SCHEMA$; }
  // Used by DatumWriter.  Applications should not call.
  public java.lang.Object get(int field$) {
    switch (field$) {
    case 0: return dtF;
    case 1: return sF;
    case 2: return lF;
    case 3: return bF1;
    case 4: return bF2;
    default: throw new org.apache.avro.AvroRuntimeException("Bad index");
    }
  }

  // Used by DatumReader.  Applications should not call.
  @SuppressWarnings(value="unchecked")
  public void put(int field$, java.lang.Object value$) {
    switch (field$) {
    case 0: dtF = (java.lang.Long)value$; break;
    case 1: sF = (java.lang.String)value$; break;
    case 2: lF = (java.lang.Long)value$; break;
    case 3: bF1 = (java.lang.Boolean)value$; break;
    case 4: bF2 = (java.lang.Boolean)value$; break;
    default: throw new org.apache.avro.AvroRuntimeException("Bad index");
    }
  }

  /**
   * Gets the value of the 'dtF' field.
   * @return myDateTime doc string
   */
  public java.lang.Long getDtF() {
    return dtF;
  }


  /**
   * Gets the value of the 'sF' field.
   * @return sField could be null
   */
  public java.lang.String getSF() {
    return sF;
  }


  /**
   * Gets the value of the 'lF' field.
   * @return lucky number
   */
  public java.lang.Long getLF() {
    return lF;
  }


  /**
   * Gets the value of the 'bF1' field.
   * @return default could be only null, so is redundant here
   */
  public java.lang.Boolean getBF1() {
    return bF1;
  }


  /**
   * Gets the value of the 'bF2' field.
   * @return default is boolean , so it could be true or false
   */
  public java.lang.Boolean getBF2() {
    return bF2;
  }


  /**
   * Creates a new HeadRecord RecordBuilder.
   * @return A new HeadRecord RecordBuilder
   */
  public static com.zgconsultants.avro.schema.HeadRecord.Builder newBuilder() {
    return new com.zgconsultants.avro.schema.HeadRecord.Builder();
  }

  /**
   * Creates a new HeadRecord RecordBuilder by copying an existing Builder.
   * @param other The existing builder to copy.
   * @return A new HeadRecord RecordBuilder
   */
  public static com.zgconsultants.avro.schema.HeadRecord.Builder newBuilder(com.zgconsultants.avro.schema.HeadRecord.Builder other) {
    return new com.zgconsultants.avro.schema.HeadRecord.Builder(other);
  }

  /**
   * Creates a new HeadRecord RecordBuilder by copying an existing HeadRecord instance.
   * @param other The existing instance to copy.
   * @return A new HeadRecord RecordBuilder
   */
  public static com.zgconsultants.avro.schema.HeadRecord.Builder newBuilder(com.zgconsultants.avro.schema.HeadRecord other) {
    return new com.zgconsultants.avro.schema.HeadRecord.Builder(other);
  }

  /**
   * RecordBuilder for HeadRecord instances.
   */
  public static class Builder extends org.apache.avro.specific.SpecificRecordBuilderBase<HeadRecord>
    implements org.apache.avro.data.RecordBuilder<HeadRecord> {

    /** myDateTime doc string */
    private long dtF;
    /** sField could be null */
    private java.lang.String sF;
    /** lucky number */
    private long lF;
    /** default could be only null, so is redundant here */
    private java.lang.Boolean bF1;
    /** default is boolean , so it could be true or false */
    private java.lang.Boolean bF2;

    /** Creates a new Builder */
    private Builder() {
      super(SCHEMA$);
    }

    /**
     * Creates a Builder by copying an existing Builder.
     * @param other The existing Builder to copy.
     */
    private Builder(com.zgconsultants.avro.schema.HeadRecord.Builder other) {
      super(other);
      if (isValidValue(fields()[0], other.dtF)) {
        this.dtF = data().deepCopy(fields()[0].schema(), other.dtF);
        fieldSetFlags()[0] = true;
      }
      if (isValidValue(fields()[1], other.sF)) {
        this.sF = data().deepCopy(fields()[1].schema(), other.sF);
        fieldSetFlags()[1] = true;
      }
      if (isValidValue(fields()[2], other.lF)) {
        this.lF = data().deepCopy(fields()[2].schema(), other.lF);
        fieldSetFlags()[2] = true;
      }
      if (isValidValue(fields()[3], other.bF1)) {
        this.bF1 = data().deepCopy(fields()[3].schema(), other.bF1);
        fieldSetFlags()[3] = true;
      }
      if (isValidValue(fields()[4], other.bF2)) {
        this.bF2 = data().deepCopy(fields()[4].schema(), other.bF2);
        fieldSetFlags()[4] = true;
      }
    }

    /**
     * Creates a Builder by copying an existing HeadRecord instance
     * @param other The existing instance to copy.
     */
    private Builder(com.zgconsultants.avro.schema.HeadRecord other) {
            super(SCHEMA$);
      if (isValidValue(fields()[0], other.dtF)) {
        this.dtF = data().deepCopy(fields()[0].schema(), other.dtF);
        fieldSetFlags()[0] = true;
      }
      if (isValidValue(fields()[1], other.sF)) {
        this.sF = data().deepCopy(fields()[1].schema(), other.sF);
        fieldSetFlags()[1] = true;
      }
      if (isValidValue(fields()[2], other.lF)) {
        this.lF = data().deepCopy(fields()[2].schema(), other.lF);
        fieldSetFlags()[2] = true;
      }
      if (isValidValue(fields()[3], other.bF1)) {
        this.bF1 = data().deepCopy(fields()[3].schema(), other.bF1);
        fieldSetFlags()[3] = true;
      }
      if (isValidValue(fields()[4], other.bF2)) {
        this.bF2 = data().deepCopy(fields()[4].schema(), other.bF2);
        fieldSetFlags()[4] = true;
      }
    }

    /**
      * Gets the value of the 'dtF' field.
      * myDateTime doc string
      * @return The value.
      */
    public java.lang.Long getDtF() {
      return dtF;
    }

    /**
      * Sets the value of the 'dtF' field.
      * myDateTime doc string
      * @param value The value of 'dtF'.
      * @return This builder.
      */
    public com.zgconsultants.avro.schema.HeadRecord.Builder setDtF(long value) {
      validate(fields()[0], value);
      this.dtF = value;
      fieldSetFlags()[0] = true;
      return this;
    }

    /**
      * Checks whether the 'dtF' field has been set.
      * myDateTime doc string
      * @return True if the 'dtF' field has been set, false otherwise.
      */
    public boolean hasDtF() {
      return fieldSetFlags()[0];
    }


    /**
      * Clears the value of the 'dtF' field.
      * myDateTime doc string
      * @return This builder.
      */
    public com.zgconsultants.avro.schema.HeadRecord.Builder clearDtF() {
      fieldSetFlags()[0] = false;
      return this;
    }

    /**
      * Gets the value of the 'sF' field.
      * sField could be null
      * @return The value.
      */
    public java.lang.String getSF() {
      return sF;
    }

    /**
      * Sets the value of the 'sF' field.
      * sField could be null
      * @param value The value of 'sF'.
      * @return This builder.
      */
    public com.zgconsultants.avro.schema.HeadRecord.Builder setSF(java.lang.String value) {
      validate(fields()[1], value);
      this.sF = value;
      fieldSetFlags()[1] = true;
      return this;
    }

    /**
      * Checks whether the 'sF' field has been set.
      * sField could be null
      * @return True if the 'sF' field has been set, false otherwise.
      */
    public boolean hasSF() {
      return fieldSetFlags()[1];
    }


    /**
      * Clears the value of the 'sF' field.
      * sField could be null
      * @return This builder.
      */
    public com.zgconsultants.avro.schema.HeadRecord.Builder clearSF() {
      sF = null;
      fieldSetFlags()[1] = false;
      return this;
    }

    /**
      * Gets the value of the 'lF' field.
      * lucky number
      * @return The value.
      */
    public java.lang.Long getLF() {
      return lF;
    }

    /**
      * Sets the value of the 'lF' field.
      * lucky number
      * @param value The value of 'lF'.
      * @return This builder.
      */
    public com.zgconsultants.avro.schema.HeadRecord.Builder setLF(long value) {
      validate(fields()[2], value);
      this.lF = value;
      fieldSetFlags()[2] = true;
      return this;
    }

    /**
      * Checks whether the 'lF' field has been set.
      * lucky number
      * @return True if the 'lF' field has been set, false otherwise.
      */
    public boolean hasLF() {
      return fieldSetFlags()[2];
    }


    /**
      * Clears the value of the 'lF' field.
      * lucky number
      * @return This builder.
      */
    public com.zgconsultants.avro.schema.HeadRecord.Builder clearLF() {
      fieldSetFlags()[2] = false;
      return this;
    }

    /**
      * Gets the value of the 'bF1' field.
      * default could be only null, so is redundant here
      * @return The value.
      */
    public java.lang.Boolean getBF1() {
      return bF1;
    }

    /**
      * Sets the value of the 'bF1' field.
      * default could be only null, so is redundant here
      * @param value The value of 'bF1'.
      * @return This builder.
      */
    public com.zgconsultants.avro.schema.HeadRecord.Builder setBF1(java.lang.Boolean value) {
      validate(fields()[3], value);
      this.bF1 = value;
      fieldSetFlags()[3] = true;
      return this;
    }

    /**
      * Checks whether the 'bF1' field has been set.
      * default could be only null, so is redundant here
      * @return True if the 'bF1' field has been set, false otherwise.
      */
    public boolean hasBF1() {
      return fieldSetFlags()[3];
    }


    /**
      * Clears the value of the 'bF1' field.
      * default could be only null, so is redundant here
      * @return This builder.
      */
    public com.zgconsultants.avro.schema.HeadRecord.Builder clearBF1() {
      bF1 = null;
      fieldSetFlags()[3] = false;
      return this;
    }

    /**
      * Gets the value of the 'bF2' field.
      * default is boolean , so it could be true or false
      * @return The value.
      */
    public java.lang.Boolean getBF2() {
      return bF2;
    }

    /**
      * Sets the value of the 'bF2' field.
      * default is boolean , so it could be true or false
      * @param value The value of 'bF2'.
      * @return This builder.
      */
    public com.zgconsultants.avro.schema.HeadRecord.Builder setBF2(java.lang.Boolean value) {
      validate(fields()[4], value);
      this.bF2 = value;
      fieldSetFlags()[4] = true;
      return this;
    }

    /**
      * Checks whether the 'bF2' field has been set.
      * default is boolean , so it could be true or false
      * @return True if the 'bF2' field has been set, false otherwise.
      */
    public boolean hasBF2() {
      return fieldSetFlags()[4];
    }


    /**
      * Clears the value of the 'bF2' field.
      * default is boolean , so it could be true or false
      * @return This builder.
      */
    public com.zgconsultants.avro.schema.HeadRecord.Builder clearBF2() {
      bF2 = null;
      fieldSetFlags()[4] = false;
      return this;
    }

    @Override
    public HeadRecord build() {
      try {
        HeadRecord record = new HeadRecord();
        record.dtF = fieldSetFlags()[0] ? this.dtF : (java.lang.Long) defaultValue(fields()[0]);
        record.sF = fieldSetFlags()[1] ? this.sF : (java.lang.String) defaultValue(fields()[1]);
        record.lF = fieldSetFlags()[2] ? this.lF : (java.lang.Long) defaultValue(fields()[2]);
        record.bF1 = fieldSetFlags()[3] ? this.bF1 : (java.lang.Boolean) defaultValue(fields()[3]);
        record.bF2 = fieldSetFlags()[4] ? this.bF2 : (java.lang.Boolean) defaultValue(fields()[4]);
        return record;
      } catch (Exception e) {
        throw new org.apache.avro.AvroRuntimeException(e);
      }
    }
  }

  private static final org.apache.avro.io.DatumWriter
    WRITER$ = new org.apache.avro.specific.SpecificDatumWriter(SCHEMA$);

  @Override public void writeExternal(java.io.ObjectOutput out)
    throws java.io.IOException {
    WRITER$.write(this, SpecificData.getEncoder(out));
  }

  private static final org.apache.avro.io.DatumReader
    READER$ = new org.apache.avro.specific.SpecificDatumReader(SCHEMA$);

  @Override public void readExternal(java.io.ObjectInput in)
    throws java.io.IOException {
    READER$.read(this, SpecificData.getDecoder(in));
  }

}
```
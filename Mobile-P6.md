---


---

<h1 id="to-perform-database-connectivity-of-android-app-using-sqlite">6. To perform database connectivity of Android app using SQLite</h1>
<p>Android SQLite is a very lightweight database which comes with Android OS. Android SQLite combines a clean SQL interface with a very small memory footprint and decent speed. For Android, SQLite is “baked into” the Android runtime, so every Android application can create its own SQLite databases.</p>
<p>Android SQLite native API is not JDBC, as JDBC might be too much overhead for a memory-limited smartphone. Once a database is created successfully its located in data/data//databases/accessible from Android Device Monitor.</p>
<p>SQLite is a typical relational database, containing tables (which consists of rows and columns), indexes etc. We can create our own tables to hold the data accordingly. This structure is referred to as a schema.</p>
<h3 id="android-sqlite-sqliteopenhelper">Android SQLite SQLiteOpenHelper</h3>
<p>Android has features available to handle changing database schemas, which mostly depend on using the SQLiteOpenHelper class.</p>
<ol>
<li>When the application runs the first time – At this point, we do not yet have a database. So we will have to create the tables, indexes, starter data, and so on.</li>
<li>When the application is upgraded to a newer schema – Our database will still be on the old schema from the older edition of the app. We will have option to alter the database schema to match the needs of the rest of the app.</li>
</ol>
<p>SQLiteOpenHelper wraps up these logic to create and upgrade a database as per our specifications. For that we’ll need to create a custom subclass of SQLiteOpenHelper implementing at least the following three methods.</p>
<h3 id="constructor">Constructor</h3>
<p>This takes the Context (e.g., an Activity), the name of the database, an optional cursor factory (we’ll discuss this later), and an integer representing the version of the database schema you are using (typically starting from 1 and increment later).</p>
<p><code>public DatabaseHelper(Context context) { super(Context, DB_Name,null, DB_Version); }</code></p>
<h4 id="oncreatedb-sqlitedatabase-db">OnCreateDB( SQLiteDatabase db)</h4>
<p>It’s called when there is no database and the app needs one. It passes us a SQLiteDatabase object, pointing to a newly-created database, that we can populate with tables and initial data.</p>
<ol>
<li>onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) : It’s called when the schema version we need does not match the schema version of the database, It passes us a SQLiteDatabase object and the old and new version numbers. Hence we can figure out the best way to convert the database from the old schema to the new one.</li>
</ol>
<p>We define a DBManager class to perform all database CRUD(Create, Read, Update and Delete) operations.</p>
<h4 id="opening-and-closing-android-sqlite-database-connection">Opening and Closing Android SQLite Database Connection</h4>
<p>Before performing any database operations like insert, update, delete records in a table, first open the database connection by calling getWritableDatabase() method as shown below:</p>
<p><code>public DBManager open() throws SQLException { dbHelper = new DatabaseHelper(context); database = dbHelper.getWritableDatabase(); return this; }</code></p>
<p>The dbHelper is an instance of the subclass of SQLiteOpenHelper.</p>
<p>To close a database connection the following method is invoked.<br>
<code>public void close() { dbHelper.close(); }</code></p>
<h3 id="inserting-new-record-into-android-sqlitedatabase-table">Inserting new Record into Android SQLitedatabase table</h3>
<p>The following code snippet shows how to insert a new record in the android SQLite database.</p>
<p><code>public void insert(String name, String desc) { ContentValues contentValue = new ContentValues(); contentValue.put(DatabaseHelper.SUBJECT, name); contentValue.put(DatabaseHelper.DESC, desc); database.insert(DatabaseHelper.TABLE_NAME, null, contentValue); }</code></p>
<p>Content Values creates an empty set of values using the given initial size. We’ll discuss the other instance values when we jump into the coding part</p>
<h3 id="updating-record-in-android-sqlite-database-table">Updating Record in Android SQLite database table</h3>
<p>The following snippet shows how to update a single record.</p>
<p><code>public int update(long _id, String name, String desc) { ContentValues contentValues = new ContentValues(); contentValues.put(DatabaseHelper.SUBJECT, name); contentValues.put(DatabaseHelper.DESC, desc); int i = database.update(DatabaseHelper.TABLE_NAME, contentValues, DatabaseHelper._ID + " = " + _id, null); return i; }</code></p>
<h3 id="android-sqlite-–-deleting-a-record">Android SQLite – Deleting a Record</h3>
<p>We just need to pass the id of the record to be deleted as shown below.</p>
<p><code>public void delete(long _id) { database.delete(DatabaseHelper.TABLE_NAME, DatabaseHelper._ID + "=" + _id, null); }</code></p>
<h3 id="android-sqlite-project-code">Android SQLite Project Code</h3>
<p><code>DatabaseHelper.java package com.journaldev.sqlite; import android.content.Context; import android.database.sqlite.SQLiteDatabase; import android.database.sqlite.SQLiteOpenHelper; public class DatabaseHelper extends SQLiteOpenHelper { // Table Name public static final String TABLE_NAME = "COUNTRIES"; // Table columns public static final String _ID = "_id"; public static final String SUBJECT = "subject"; public static final String DESC = "description"; // Database Information static final String DB_NAME = "JOURNALDEV_COUNTRIES.DB"; // database version static final int DB_VERSION = 1; // Creating table query private static final String CREATE_TABLE = "create table " + TABLE_NAME + "(" + _ID+ " INTEGER PRIMARY KEY AUTOINCREMENT, " + SUBJECT + " TEXT NOT NULL, " + DESC + " TEXT);"; public DatabaseHelper(Context context) { super(context, DB_NAME, null, DB_VERSION); } @Override public void onCreate(SQLiteDatabase db) { db.execSQL(CREATE_TABLE); } @Override public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) { db.execSQL("DROP TABLE IF EXISTS " + TABLE_NAME); onCreate(db); } }</code></p>
<pre><code></code></pre>


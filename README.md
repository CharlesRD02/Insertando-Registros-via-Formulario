//Main Activity
package com.example.registrosfor

import BaseDatosHelper
import android.content.ContentValues
import android.os.Bundle
import android.view.View
import android.widget.EditText
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    private lateinit var txtCodigo: EditText
    private lateinit var txtDescripcion: EditText
    private lateinit var txtPrecio: EditText
    private lateinit var dbHelper: BaseDatosHelper

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        txtCodigo = findViewById(R.id.txtCodigo)
        txtDescripcion = findViewById(R.id.txtDescripcion)
        txtPrecio = findViewById(R.id.txtPrecio)
        dbHelper = BaseDatosHelper(this, "tienda", null, 1)
    }

    fun insertar(view: View) {
        val baseDatos = dbHelper.writableDatabase

        val codigo = txtCodigo.text.toString()
        val descripcion = txtDescripcion.text.toString()
        val precio = txtPrecio.text.toString()

        if (codigo.isNotEmpty() && descripcion.isNotEmpty() && precio.isNotEmpty()) {
            val registro = ContentValues().apply {
                put("codigo", codigo)
                put("descripcion", descripcion)
                put("precio", precio)
            }
            baseDatos.insert("productos", null, registro)
            baseDatos.close()

            txtCodigo.setText("")
            txtDescripcion.setText("")
            txtPrecio.setText("")
            Toast.makeText(this, "Se ha insertado exitosamente", Toast.LENGTH_LONG).show()
        } else {
            Toast.makeText(this, "Los campos deben tener texto", Toast.LENGTH_LONG).show()
        }
    }
}

//themes.xml
<?xml version="1.0" encoding="utf-8"?>
<resources>

    <style name="Theme.RegistrosFor" parent="Theme.AppCompat.Light.DarkActionBar"/>
</resources>

//AndroidManifest.xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light"
        tools:targetApi="31">

        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.AppCompat.Light">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>

</manifest>


//BaseDatosHelper
import android.content.Context
import android.database.sqlite.SQLiteDatabase
import android.database.sqlite.SQLiteOpenHelper

class BaseDatosHelper(
    context: Context,
    name: String,
    factory: SQLiteDatabase.CursorFactory?,
    version: Int
) : SQLiteOpenHelper(context, name, factory, version) {

    override fun onCreate(db: SQLiteDatabase?) {
        db?.execSQL(
            "CREATE TABLE IF NOT EXISTS productos (" +
                    "codigo INTEGER PRIMARY KEY, " +
                    "descripcion TEXT NOT NULL, " +
                    "precio REAL NOT NULL)"
        )
    }

    override fun onUpgrade(db: SQLiteDatabase?, oldVersion: Int, newVersion: Int) {
        db?.execSQL("DROP TABLE IF EXISTS productos")
        onCreate(db)
    }
}

//activity_main.xml
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/txtCodigo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="308dp"
        android:ems="10"
        android:hint="@string/codigo"
        android:inputType="number"
        android:minHeight="48dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/txtDescripcion"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="28dp"
        android:ems="10"
        android:hint="@string/descripcion"
        android:inputType="text"
        android:minHeight="48dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/txtCodigo" />

    <EditText
        android:id="@+id/txtPrecio"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:ems="10"
        android:hint="@string/precio"
        android:inputType="numberDecimal"
        android:minHeight="48dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/txtDescripcion" />

    <ImageButton
        android:id="@+id/imageButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="60dp"
        android:contentDescription="@string/todo"
        android:onClick="insertar"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.484"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/txtPrecio"
        app:srcCompat="@android:drawable/ic_input_add" />

</androidx.constraintlayout.widget.ConstraintLayout>

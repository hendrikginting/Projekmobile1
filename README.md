# Projekmobile1
package com.uas;

import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;
import android.util.Log;

public class DataHelperuas extends SQLiteOpenHelper {
    
	private static final String DATABASE_NAME = "nilai.db";
	private static final int DATABASE_VERSION = 1;
	
	public DataHelperuas (Context context){
		super(context, DATABASE_NAME, null, DATABASE_VERSION);
	}
	
    @Override
    public void onCreate(SQLiteDatabase db) {
        String sql = "CREATE TABLE nilai(nirm integer primary key,"
        		+"nama text null, mk text null, nilai text null);";
        Log.d("Data", "onCreate : " + sql);
        db.execSQL(sql);
        
        sql = "INSERT INTO nilai (nirm, nama, mk, nilai) VALUES ('2018020836', "
        		+ "'Hendrik Kurniawan Ginting, 'Pemrograman Mobile1', '75');";
        db.execSQL(sql);

    }
    
    @Override
    public void onUpgrade (SQLiteDatabase db, int arg1, int arg2) {
    	
    }
    
}


package com.uas;

import com.uas.UASActivity;
import com.uas.R;
import com.uas.TambahData;

import android.app.Activity;
import android.os.Bundle;
import android.content.Intent;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.view.View;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;

public class UASActivity extends Activity {
    /** Called when the activity is first created. */
	
	String [] daftar;
	ListView lvData;
	Cursor cursor;
	DataHelperuas dbCenter;
	public static UASActivity ma;
	
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        
        Button btnTambah = (Button) findViewById(R.id.button1);
        btnTambah.setOnClickListener(new View.OnClickListener() {
			
			@Override
			public void onClick(View arg0) {
				// TODO Auto-generated method stub
				Intent inten = new Intent (UASActivity.this, TambahData.class);
				startActivity(inten);
			}
		});
         
        ma = this;      
        dbCenter = new DataHelperuas (this);
        RefreshList ();
        
    }
    
    public void RefreshList (){
    	SQLiteDatabase db = dbCenter.getReadableDatabase();
    	cursor = db.rawQuery("SELECT * FROM nilai", null);
    	daftar = new String[cursor.getCount()];
    	cursor.moveToFirst();
    	
    	for (int i = 0; i < cursor.getCount(); i++){
    		cursor.moveToPosition(i);
    		daftar [i] = cursor.getString(1).toString();
    	
    	}
    	
    	lvData = (ListView) findViewById (R.id.listView1);
    	lvData.setAdapter(new ArrayAdapter <String>(this, android.R.layout.simple_list_item_1, daftar));
    	
    }
}

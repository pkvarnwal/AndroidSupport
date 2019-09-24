package com.example.imagefromgallery;

import android.content.Context;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.EditText;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

public class LoginActivity extends AppCompatActivity implements View.OnClickListener {

    Button buttonLogin;
    EditText editUser;
    EditText editPassword;
    CheckBox checkBox;
    public static final String MyPREFERENCES = "MyPrefs" ;
    public static final String USER = "user";
    SharedPreferences sharedpreferences;


    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);

        buttonLogin = findViewById(R.id.button_login);
        editUser = findViewById(R.id.edit_user);
        editPassword = findViewById(R.id.edit_password);
        checkBox = findViewById(R.id.checkbox);
        checkBox.setOnClickListener(this);

        buttonLogin.setOnClickListener(this);
        sharedpreferences = getSharedPreferences(MyPREFERENCES, Context.MODE_PRIVATE);
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.button_login:
// To fetch Data
                editUser.setText(sharedpreferences.getString("USER", ""));
                editPassword.setText(sharedpreferences.getString("PASSWORD", ""));
                break;

            case R.id.checkbox:
// To store Data
                SharedPreferences.Editor editor = sharedpreferences.edit();
                editor.putString("USER", editUser.getText().toString());
                editor.putString("PASSWORD", editPassword.getText().toString());
                editor.apply();
                Toast.makeText(this, "Thanks", Toast.LENGTH_SHORT).show();
                break;
        }
    }
}


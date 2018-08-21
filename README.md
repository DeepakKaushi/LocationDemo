# LocationDemo
location
package com.example.vishcomputers.mydemo;

import android.annotation.SuppressLint;
import android.content.pm.PackageManager;
import android.icu.text.SimpleDateFormat;
import android.location.Address;
import android.location.Geocoder;
import android.location.Location;
import android.os.CountDownTimer;
import android.support.v4.app.ActivityCompat;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import com.google.android.gms.location.FusedLocationProviderClient;
import com.google.android.gms.location.LocationCallback;
import com.google.android.gms.location.LocationRequest;
import com.google.android.gms.location.LocationResult;
import com.google.android.gms.location.LocationServices;
import com.google.android.gms.tasks.OnSuccessListener;

import java.sql.Time;
import java.text.DateFormat;
import java.util.Calendar;
import java.util.Date;
import java.util.List;
import java.util.Locale;
import java.util.Timer;
import java.util.TimerTask;

import static android.Manifest.permission.ACCESS_FINE_LOCATION;
import static java.util.Locale.getDefault;

public class Main2Activity extends AppCompatActivity {

    private Button start, stop;
    private FusedLocationProviderClient client;
    private TextView latt, longg, addr, date;
    private Geocoder geocoder;
    private List<Address> addresses;
    double lat,lan;
    long counter,result;
    LocationRequest request;
    @SuppressLint("RestrictedApi")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
        requestPermision();
        start = (Button) findViewById(R.id.start);
        latt = (TextView) findViewById(R.id.text);
        longg = (TextView) findViewById(R.id.txt);
        addr = (TextView) findViewById(R.id.txtadd);
        date = (TextView) findViewById(R.id.date);

        stop = (Button) findViewById(R.id.stop);
        client = LocationServices.getFusedLocationProviderClient(Main2Activity.this);
        request = new LocationRequest();
        request.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);
        request.setFastestInterval(1000);
        request.setInterval(40000);

        start.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {


                if (ActivityCompat.checkSelfPermission(Main2Activity.this, ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(Main2Activity.this, android.Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {

                    return;
                }
                client.requestLocationUpdates(request,new LocationCallback(){
                    public void onLocationResult(LocationResult locationResult){
                        super.onLocationResult(locationResult);
                        lat = locationResult.getLastLocation().getLatitude();
                        lan = locationResult.getLastLocation().getLongitude();
                        geocoder = new Geocoder(getApplicationContext(), Locale.getDefault());
                        try {
                            addresses = geocoder.getFromLocation(lat, lan, 1);
                            String address = addresses.get(0).getAddressLine(0);
                            addr.setText(address);
                            latt.setText(String.valueOf(lat));
                            longg.setText(String.valueOf(lan));
                           // String current = DateFormat.getDateTimeInstance().format(new Date());
                           // date.setText(current);
                           counter=System.currentTimeMillis();
                           int milli=(int) counter;
                           int sec=(int)counter/1000;
                           int minute=(int)result/60;
                            date.setText(String.format("%d",minute));





                        } catch (Exception e) {
                            e.printStackTrace();
                        }

                    }
                },getMainLooper());



            }
        });


    }


    private void requestPermision() {
        ActivityCompat.requestPermissions(Main2Activity.this, new String[]{ACCESS_FINE_LOCATION}, 1);
    }
    public void onBackpress(){
        finish();
    }


}





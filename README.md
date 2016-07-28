package com.example.getip;

import java.net.InetAddress;
import java.net.NetworkInterface;
import java.net.SocketException;
import java.util.Enumeration;

import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.os.Bundle;
import android.app.Activity;
import android.app.AlertDialog;
import android.content.DialogInterface;
import android.util.Log;
import android.view.Menu;
import android.widget.TextView;

public class GetIP extends Activity {
private TextView name;
private TextView ip;
public ConnectivityManager connectivityManager;
public NetworkInfo networkInfo;
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_get_ip);
        name=(TextView)findViewById(R.id.nameTextView);
        ip=(TextView)findViewById(R.id.ipTextView);
        connectivityManager=(ConnectivityManager)getSystemService(CONNECTIVITY_SERVICE);
        networkInfo=connectivityManager.getActiveNetworkInfo();
        if (networkInfo==null) {
			myDialog();
		}
        else {
			setUpInfo();
		}
    }

//    @Override
//    public boolean onCreateOptionsMenu(Menu menu) {
//        getMenuInflater().inflate(R.menu.activity_get_ip, menu);
//        return true;
//    }
    public String getLocalIPAddress(){
    	try {
			for(Enumeration<NetworkInterface>mEnumeration=NetworkInterface.getNetworkInterfaces();mEnumeration.hasMoreElements();){
				NetworkInterface intf=mEnumeration.nextElement();
				for(Enumeration<InetAddress> enumIPAddr=intf.getInetAddresses();enumIPAddr.hasMoreElements();){
					InetAddress inetAddress=enumIPAddr.nextElement();
					if (!inetAddress.isLoopbackAddress()) {
						return inetAddress.getHostAddress().toString();
					}
				
				}
			}
		} catch (SocketException ex) {
			Log.e("ERROR",ex.toString());
		}
    	return null;
    }
    public void setUpInfo(){
    	if (networkInfo.getType()==connectivityManager.TYPE_WIFI) {
			name.setText("网络类型：WIFI");
			ip.setText("IP地址："+getLocalIPAddress());
		}
    	else if (networkInfo.getType()==connectivityManager.TYPE_MOBILE) {
			name.setText("网络类型：手机");
			ip.setText("IP类型："+getLocalIPAddress());
		}
    	else {
			name.setText("网络类型：未知");
			ip.setText("IP地址：未知");
		}
    }
    private void myDialog(){
    	AlertDialog mDialog=new AlertDialog.Builder(GetIP.this)
    	.setTitle("注意")
    	.setMessage("当前网络不可用")
    	.setPositiveButton("确定", new DialogInterface.OnClickListener() 
    	{
			
			public void onClick(DialogInterface dialog, int which) {
				// TODO Auto-generated method stub
				dialog.dismiss();
				GetIP.this.finish();
			}
		} 	)	
		.create();
    	mDialog.show();

    }
}

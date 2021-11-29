**RecycleViewAdapter.Java**
(iznad mejn aktiviti desni klik nova klasa i ime) 


    package com.example.treeapp;
    
    import android.app.AlertDialog;
    import android.content.Context;
    import android.view.LayoutInflater;
    import android.view.View;
    import android.view.ViewGroup;
    import android.widget.ImageView;
    import android.widget.TextView;
    
    import androidx.annotation.NonNull;
    import androidx.cardview.widget.CardView;
    import androidx.recyclerview.widget.RecyclerView;
    
    import com.bumptech.glide.Glide;
    import com.bumptech.glide.RequestManager;
    import com.example.treeapp.model.Treemodel;
    
    import org.w3c.dom.Text;
    
    import java.util.ArrayList;
    import java.util.zip.Inflater;
    
    public class RecycleViewAdapter extends RecyclerView.Adapter<RecycleViewAdapter.ViewHolder> {
    
        private ArrayList<Treemodel> drvece;
        private Context kontekst;
    
        public RecycleViewAdapter(Context kontekst, ArrayList<Treemodel> drvece) {
            this.drvece = drvece;
            this.kontekst = kontekst;
        }
    
        @NonNull
        @Override
        public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
            LayoutInflater inflater = LayoutInflater.from(parent.getContext());
            View viewCard = inflater.inflate(R.layout.card_layout, parent, false);
            ViewHolder holder = new ViewHolder(viewCard);
            return holder;
        }
    
        @Override
        public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
            holder.imeDrveta.setText(drvece.get(position).ime);
            holder.latinskiNaziv.setText(drvece.get(position).latinskiNaziv);
            RequestManager glideManager = Glide.with(kontekst);
            glideManager.load(drvece.get(position).slika1).into(holder.slikaDrveta);
            holder.card.setTag(position);
    
            holder.card.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    LayoutInflater inflater = LayoutInflater.from(kontekst);
                    View viewDialog = inflater.inflate(R.layout.dialog_image, null, true);
                    Integer posi = (Integer)view.getTag();
                    ImageView slikaLisca = viewDialog.findViewById(R.id.slika2);
                    Glide.with(kontekst)
                            .load(drvece.get(posi.intValue()).slika2)
                            .into(slikaLisca);
                    AlertDialog.Builder builder = new AlertDialog.Builder(kontekst);
                    builder.setView(viewDialog);
                    builder.show();
                }
            });
    
        }
    
        @Override
        public int getItemCount() {
            return drvece.size();
        }
    
    
        public class ViewHolder extends RecyclerView.ViewHolder {
    
            TextView imeDrveta;
            TextView latinskiNaziv;
            ImageView slikaDrveta;
            CardView card;
    
            public ViewHolder(@NonNull View itemView) {
                super(itemView);
                imeDrveta = itemView.findViewById(R.id.ime);
                latinskiNaziv = itemView.findViewById(R.id.latinski);
                slikaDrveta = itemView.findViewById(R.id.slika1);
                card = itemView.findViewById(R.id.card);
            }
        }
    
    }


**Dialog_image.xml**
(lejaut desni klik novi resurs fajl, rut framelayout) 

    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
    
        <ImageView
            android:id="@+id/slika2"
            android:layout_width="match_parent"
            android:layout_height="300dp"
            android:layout_margin="7dp"
            android:scaleType="centerCrop"
            app:srcCompat="@drawable/ic_launcher_background" />
    
    </FrameLayout>

**Main_Activity.java**
(ovo ima) 

    package com.example.treeapp;
    
    import androidx.appcompat.app.AppCompatActivity;
    import androidx.recyclerview.widget.GridLayoutManager;
    import androidx.recyclerview.widget.RecyclerView;
    
    import android.os.Bundle;
    
    import com.example.treeapp.model.Treemodel;
    
    import java.util.ArrayList;
    
    public class MainActivity extends AppCompatActivity {
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            ArrayList<Treemodel> drvece = getData();
    
            RecycleViewAdapter adapter = new RecycleViewAdapter(this, drvece);
    
            RecyclerView recyclerView = findViewById(R.id.recycler);
            recyclerView.setAdapter(adapter);
    
            recyclerView.setLayoutManager(new GridLayoutManager(this, 2));
    
        }
    
        private ArrayList<Treemodel> getData() {
            ArrayList<Treemodel> d = new ArrayList<>();
            d.add(new Treemodel("Pine","Pinus sylvestris","https://yopadoc.com/trees/bor1.jfif","https://yopadoc.com/trees/bor2.jfif"));
            d.add(new Treemodel("Linden","Tilia cordata","https://yopadoc.com/trees/lipa1.jfif","https://yopadoc.com/trees/lipa2.jfif"));
            d.add(new Treemodel("Willow","Salix babylonica","https://yopadoc.com/trees/vrba1.jfif","https://yopadoc.com/trees/vrba2.jfif"));
            d.add(new Treemodel("Maple","Acer platanoides","https://yopadoc.com/trees/javor1.jfif","https://yopadoc.com/trees/javor2.jfif"));
            d.add(new Treemodel("Poplar","Populus nigra","https://yopadoc.com/trees/jablan1.jfif","https://yopadoc.com/trees/jablan2.jfif"));
            d.add(new Treemodel("Locust","Robinia pseudoacacia","https://yopadoc.com/trees/bagrem1.jfif","https://yopadoc.com/trees/bagrem2.jfif"));
            return d;
        }
    
    }

**activity_main.xml**
(i ovo ima) 

    <?xml version="1.0" encoding="utf-8"?>
    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">
    
        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recycler"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
    </androidx.constraintlayout.widget.ConstraintLayout>

**TreeModel.java**
(iznad mejn aktiviti desni klik pekidz model i na njega desni klik nova klasa ime i code generate constructor) 

    package com.example.treeapp.model;
    
    public class Treemodel {
    
        public String ime;
        public String latinskiNaziv;
        public String slika1;
        public String slika2;
    
        public Treemodel(String ime, String latinskiNaziv, String slika1, String slika2) {
            this.ime = ime;
            this.latinskiNaziv = latinskiNaziv;
            this.slika1 = slika1;
            this.slika2 = slika2;
        }
    }

**card_layout**
(lejaut desni klik novi resurs fajl rut Card.View)

    <?xml version="1.0" encoding="utf-8"?>
    <androidx.cardview.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="175dp"
        android:layout_height="250dp"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        app:cardCornerRadius="20dp"
        app:cardElevation="10dp"
        android:layout_marginTop="10dp"
        android:layout_marginHorizontal="5dp"
        android:id="@+id/card"
        >
    
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical"
            android:padding="15dp">
    
            <TextView
                android:id="@+id/ime"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAlignment="center"
                android:textStyle="bold"
                android:layout_marginVertical="5dp"
                android:textSize="18sp"/>
    
            <TextView
                android:id="@+id/latinski"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAlignment="center"
                android:textStyle="italic"
                android:layout_marginBottom="5dp"
                android:textSize="14sp"/>
    
            <ImageView
                android:id="@+id/slika1"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:contentDescription="drvo"
                app:srcCompat="@drawable/ic_launcher_background" />
    
        </LinearLayout>
    
    </androidx.cardview.widget.CardView>



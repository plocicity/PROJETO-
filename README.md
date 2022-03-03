ublic class JavaFragment extends Fragment {

    // Null until onCreateView.
    private Button button;

    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, ViewGroup container,
            Bundle savedInstanceState) {
        View root = inflater.inflate(R.layout.fragment_content, container,false);

        // Get a reference to the button in the view, only after the root view is inflated.
        button = root.findViewById(R.id.button);

        return root;
    }

    @Override
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);

        // Not null at this point of time when onViewCreated runs
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                ...
            }
        });
    }
    
    
    <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="br.com.gilbercs.messenger">

    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Messenger">
        <activity
            android:name=".chat.ChatConversaActivity"
            android:label="@string/app_name"
            android:parentActivityName=".chat.ChatUserActivity"
            android:exported="true" />
        <activity
            android:name=".chat.ChatUserActivity"
            android:label="@string/title_select_user"
            android:parentActivityName=".chat.ChatHomeActivity"
            android:exported="true" />
        <activity
            android:name=".firebase.LoginActivity"
            android:exported="true" />
        <activity
            android:name=".firebase.RegisterActivity"
            android:exported="true"
            android:label="@string/title_registro"
            android:parentActivityName=".firebase.LoginActivity" />
        <activity
            android:name=".chat.ChatHomeActivity"
            android:icon="@drawable/ic_message_24"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest> 
    
    lass AdapterHomeConversa(val modelChat: ModelChat): Item<ViewHolder>() {
    //declaração de variavel
    var modelUser: ModelUser?=null
    override fun bind(viewHolder: ViewHolder, position: Int) {
        //mensagens do usuario no bate-papo
        viewHolder.itemView.id_adapter_home_messages_recentes.text = modelChat.text
        //id do usuario
        val userPartnerId: String
        if (modelChat.oneId == FirebaseAuth.getInstance().uid){
            userPartnerId = modelChat.twoId
        }else{
            userPartnerId = modelChat.oneId
        }
        val database = FirebaseDatabase.getInstance().getReference("/Users/$userPartnerId")
        database.addListenerForSingleValueEvent(object: ValueEventListener{
            override fun onDataChange(snapshot: DataSnapshot) {
                modelUser = snapshot.getValue(ModelUser::class.java)
                viewHolder.itemView.id_adapter_home_user_name.text = modelUser!!.name
                val imgPerfil = viewHolder.itemView.id_adapter_home_img_perfil
               Picasso.get().load(modelUser!!.urlImg).into(imgPerfil)
            }

            override fun onCancelled(error: DatabaseError) {
                TODO("Not yet implemented")
            }

        })
    }

    override fun getLayout(): Int {
        return R.layout.adapter_home_conversa
    }

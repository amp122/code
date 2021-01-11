public class data extends AppCompatActivity {
    private EditText eText;
    private ImageButton sBtn;
    private RecyclerView mRes;
    private DatabaseReference mUserDatabase;
    private TextView stat;
    private ImageView imageView;
    private TextView tname;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_data);

        mUserDatabase = FirebaseDatabase.getInstance ().getReference ("Users");
        eText = (EditText)findViewById (R.id.sText);
        sBtn = (ImageButton) findViewById (R.id.sbtn);
        mRes = (RecyclerView) findViewById (R.id.rView);
        mRes.setHasFixedSize (true);
        mRes.setLayoutManager (new LinearLayoutManager (this));
        stat = (TextView) findViewById (R.id.stat);
        imageView = (ImageView) findViewById (R.id.imageview1);
        tname = (TextView) findViewById (R.id.textname);

        sBtn.setOnClickListener (new View.OnClickListener () {
            @Override
            public void onClick(View v) {

                firebaseUserSearch();

            }
        });

         }

    private void firebaseUserSearch() {


        FirebaseRecyclerAdapter<Users, UsersViewHolder> firebaseRecyclerAdapter = new FirebaseRecyclerAdapter<Users, UsersViewHolder>(
                Users.class,
                R.layout.list_layout,
                UsersViewHolder.class,
                mUserDatabase


        ) {
            @Override
            protected void onBindViewHolder(@NonNull UsersViewHolder holder, int position, @NonNull Users model) {

                holder.SetDetails (getApplicationContext (),model.getName (), model.getStat (), model.getImage ());

            }

            @NonNull
            @Override
            public UsersViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
                return null;
            }
        };

        mRes.setAdapter (firebaseRecyclerAdapter);

    }

    //View Holder Class
         public static class UsersViewHolder extends RecyclerView.ViewHolder {

             View mView;

             public UsersViewHolder(@NonNull View itemView) {
                 super (itemView);

                 mView = itemView;
             }

             public void SetDetails(Context ctx, String userName, String userStat, String userImage){

                 TextView NAME =(TextView) mView.findViewById (R.id.textname);
                 TextView STAT = (TextView) mView.findViewById (R.id.stat);
                 ImageView IMAGE = (ImageView) mView.findViewById (R.id.imageview1);

                NAME.setText (userName);
                STAT.setText (userStat);
                Glide.with (ctx).load (userImage).into (IMAGE);

             }
         }
}

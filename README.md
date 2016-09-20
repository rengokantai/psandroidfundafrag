### psandroidfundafrag
####3 Adding the Fragment to an Activity
#####Adding a Fragment to an Activity by XML
create project->empty activity  
create new java class under java
```
public classs KFragment extends Fragment{
//extend android.app.Fragment (not support.v4)

  @Nullable
  @Override
  public View onCreateView(LayoutInflater inflator, ViewGroup container, Bundle savedInstanceState){
    //return super.onCreateView(inflater,container,savedInstanceState);
    View view = inflater.inflate(R.layout.fragment_ke,container,false);
    return view;
  }
}
```
in activity_main.xml, design mode, select custom->KFragment,->click,select @layout/fragment_k

#####3. Getting Familiar with FragmentManager and FragmentTransaction
FragmentManager - Interface to interact with Fragment objects inside the Activity

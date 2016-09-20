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
FragmentTransaction -API for performing a set of Fragment opeaations such as add, remove, replace
#####4. Adding a Fragment to an Activity Programmatically by JAVA
steps:
- create subclass of Fragment
- create a layout for Fragment
- link layout with fragment subclass
- place the fragment inside an activity
  - initialize fragmentManager
  - initilize fragmentTransaction
  - start add/remove/replace operation
  - commit the transaction  

MainActivity.java
```
...
KFragment kf = new KFragment();
FragmentManager fm = getFragmentManager();
FragementTransaction t = manager.beginTransaction();
t.add(R.id.main,kf,"kFragment");  //main = MainActivity's layout id
transaction.commit();
```
center a fragment
```
<FrameLayout android:id="@+id/container" android:layout_width="wrap_content" android:layout_height="wrap_content" android:layout_centerInParent="true"/>
```
change previous code
```
t.add(R.id.container,kf,"kFragment");  //main = MainActivity's layout id
```

####Exploring Fragment Lifecycle

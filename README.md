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
    return super.onCreateView(inflater,container,savedInstanceState);
  }
}
```

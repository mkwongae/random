## Welcome to droid

Challange done on Genymotion virtual device

Install the apk with adb:
```
adb install -t <file>
```

By decompiling the file we got 4 Java classes.  

The first activity asks for a string and passes it to next activity:

```
class C08502 implements OnClickListener {
    final /* synthetic */ MainActivity f3094a;

    C08502(MainActivity mainActivity) {
        this.f3094a = mainActivity;
    }

    public void onClick(View view) {
        String obj = this.f3094a.f3095l.getText().toString();
        int length = this.f3094a.f3095l.length();
        if (length >= 10 && length <= 26) {
            Intent intent = new Intent(view.getContext(), Main2Activity.class);
            intent.putExtra("edittext", obj);
            this.f3094a.startActivity(intent);
        }
    }
}
```

The second activity asks for another string. This time it checks my input in the function Main2Activity.m4831a():

```
public static String m4831a(String str) {
    int i = 0;
    String str2 = "";
    char[] cArr = new char[]{'c', 'o', 'd', 'e', 'g', 'a', 't', 'e', '2', '0', '1', '8', 'h', 'u', 'r', 'r', 'a', 'y', '!', 'H', 'A', 'H', 'A', 'H', 'A', 'L', 'O', 'L'};
    int length = str.length();
    int i2 = 0;
    String str3 = "";
    while (i2 < length) {
        int i3;
        int charAt = str.charAt(i2) ^ String.valueOf(cArr).charAt(length + i2);
        for (i3 = 0; i3 < cArr.length - 1; i3++) {
            if (String.valueOf(charAt).equals(Character.valueOf(String.valueOf(cArr).charAt(i3)))) {
                charAt = new Random().nextInt();
            }
        }
        i3 = 0;
        while (i3 < charAt) {
            i3 = (i3 + i3) + 1;
        }
        i2++;
        str3 = str3 + String.valueOf(i3);
    }
    String str4 = str2;
    while (i < length) {
        str4 = str4 + (str3.charAt(i) ^ i);
        i++;
    }
    return str4;
}

public void onClick(View view) {
    String obj = this.f3086b.f3087l.getText().toString();
    String str = "";
    if (Main2Activity.m4831a(stringExtra).equals(obj)) {
        Intent intent = new Intent(view.getContext(), Main3Activity.class);
        intent.putExtra("id", stringExtra);
        intent.putExtra("pass", obj);
        this.f3086b.startActivity(intent);
        return;
    }
    ((TextView) this.f3086b.findViewById(R.id.sample_text)).setText("wrong! try again");
}
```

The third activity asks for our input again. The input is also checked before going to the next activity. People said it is impossible to find a valid input but I did not drill into the details at that time.

```
class C08472 implements OnClickListener {
    final /* synthetic */ Main3Activity f3089a;

    C08472(Main3Activity main3Activity) {
        this.f3089a = main3Activity;
    }

    public void onClick(View view) {
        if (this.f3089a.m4832k().equals(this.f3089a.f3090l.getText().toString())) {
            this.f3089a.startActivity(new Intent(view.getContext(), Main4Activity.class));
            return;
        }
        ((TextView) this.f3089a.findViewById(R.id.sample_text)).setText("wrong! try again");
    }
}
```

The final one makes a native function call:

```
protected void onCreate(Bundle bundle) {
    super.onCreate(bundle);
    setContentView((int) R.layout.activity_main4);
    m2632a((Toolbar) findViewById(R.id.toolbar));
    ((FloatingActionButton) findViewById(R.id.fab)).setOnClickListener(new C08481(this));
    System.loadLibrary("native-lib");
    this.f3092l = (EditText) findViewById(R.id.editText);
    this.f3092l.setText(stringFromJNI());
}
```

To solve this challange I used [Inspeckage](http://ac-pm.github.io/Inspeckage/) to hook the two input checking functions and replace the return values with 1.

FLAG{W3_w3r3_Back_70_$3v3n7een!!!}


# WLB-list

<img src="https://user-images.githubusercontent.com/109572328/208228163-a5b0613c-0699-4637-aafb-3d8c2a443bfd.jpeg" />

<h3>사용한 언어</h3>
<ol>
  <li>React Native</li>
</ol>

<h3>구현 목표</h3>
<ol>
  <li>버튼에 따라 다른 리스트 보여주기</li>
  <li>사용자가 입력한 텍스트 받기</li>
  <li>AsyncStorage 사용해서 리스트 저장하기</li>
</ol>

<h3>제작 기간</h3>
<ul>
  <li>22.07.12 - 22.07.14</li>
</ul>

<h3>기본 정보</h3>
리액트 네이티브란

→ ios와 안드로이드에서 동작하는 네이티브 모바일 앱을 만드는 자바스크립트 프레임워크로

사용자 인터페이스를 만드는 페이스북의 자바스크립트 라이브러리인 리액트에 기반을 두고 있다.

즉, 익숙한 자바스크립트 라이브러리를 이용하면서 겉모습과 실제 동작까지 네이티브인 모바일 앱을 만들 수 있다.

exop란

→ JavaScript나 TypeScript로 iOS, Android, Web 앱을 개발할 수 있는 프레임워크이다.

앱에 필요한 다양한 인프라를 Java, xcode 등을 통해 컴파일 해준다. JS 코드를 변경할 수 있게 도와주고 컴퓨터에서 핸드폰으로 코드를 보낼 수 있게 해준다.

즉, 컴파일 과정을 건너뛰게 해준다.

<h3>코드 정리</h3>
work lift balance list app 이라는 취지에 맞게 work, travel 버튼을 만들어 줬습니다.<br>
클릭 시 깜빡이는 효과를 주기 위해 TouchableOpacity를 사용했습니다.<br>
header 스타일링을 위한 View -> TouchableOpacity -> btn text 

```
<View style={styles.header}>
  <TouchableOpacity>
    <Text style={styles.btnText}>
      Work
    </Text>
  </TouchableOpacity>
  
  <TouchableOpacity>
    <Text style={styles.btnText,}>
      Travel
    </Text>
  </TouchableOpacity>
</View>
```

work에 Todo가 있는지 없는지 알려주는 state를 만들고 기본값으로 false를 주고 work, travel 버튼에 사용했습니다.

```
const [working, setWorking] = useState(true);

const travel = () => setWorking(false);
const work = () => setWorking(true);
```

...을 사용해서 기존의 스타일에 새로운 스타일을 추가해줬습니다.

```
<Text style={{ ...styles.btnText, color: working ? "white" : theme.grey }}>
  Work
</Text>

<Text style={{ ...styles.btnText, color: !working ? "white" : theme.grey }}>
 travel
</Text>
```

Todo를 입력받기 위해 Textinput을 사용했고 Textinput에 어떤 텍스트가 입력됐는지 알려주는 onChangeText 함수를 만들었습니다.

```
const onChangeText = (payload) => (payload);

<Textinput onChangeText={onChangeText} />
```

사용자가 쓴 텍스트를 저장하기 위한 state를 만들고 기본값을 빈 문자열로 줬습니다.<br>
이 컴포넌트를 제어하기 위해 TextInput에 값을 주고 사용자가 뭔가를 입력하면 setText가 실행되도록 했습니다.

```
const [text, setText] = useState("");

const onChangeText = (payload) => setText(payload);

<Textinput onChangeText={onChangeText} value={text} />
```

제출 버튼을 누르면 onSubmitEditing 이벤트가 발생되므로 TextInput에 onSubmitEditing으로 제출 이벤트를 막아주고 returnKeyType으로 제출 버튼의 내용을 바꿔줬습니다.

```
<TextInput
  onSubmitEditing={addToDo}
  onChangeText={onChangeText}
  returnKeyType="done"
  value={text}
  placeholder={working ? "Add a To Do " : "Where do you want to go?"}
  style={styles.input}
/>
```

addToDo 함수는 text가 비었다면 그대로 반환하고 text가 있다면 새로운 오브젝트를 만들도록 했고, …을 이용해서 기존 toDos에 담겨 있던 content와 state에 새롭게 추가된 요소들을 결합해서 만들어 줬습니다.<br>
Date.now는 date를 밀리초로 계산한 값을 key로 가지고 오는 또 다른 오브젝트로 {text, working}은 오브젝트의 바디부분이고 text와 working의 true false 값을 state에서 받아오게 했습니다.<br> → key를 통해 toDos를 찾기 위한 과정<br>
addToDo 함수를 실행할 때 기존 toDos랑 새로운 toDos를 동일한 구조 안에 만들고 그 오브젝트를 state에 저장하고 saveToDos 함수를 호출하도록 했습니다. 그리고 toDo의 value를 빈값으로 설정해줬습니다.

```
const addToDo = () => {
  if (text === "") {
    return;
  }
  const newToDos = { ...toDos, [Date.now()]: { text, working } };
  setToDos(newToDos);
  setText("");
  };
```

ScrollView를 사용해서 Todo가 많으면 스크롤 할 수 있도록 했습니다.<br>
Todo를 프린트하기 위해 object.key(object)를 사용해서 key들의 배열을 얻고 key 값에 map을 사용해서 Text에 key 값의 text를 보여주도록 했습니다.

```
<ScrollView>
  {Object.keys(toDos).map((key) =>
    <View style={styles.toDo} key={key}>
      <Text style={styles.toDoText}>{toDos[key].text}</Text>
    </View>
  }
</ScrollView>
```

현재 있는 곳과 working을 비교해서 맞다면 Todo를 보여주고 아니라면 보여주지 않도록 조건문을 사용했습니다.

```
<ScrollView>
  {toDos &&
    Object.keys(toDos).map((key) =>
      toDos[key].working === working ? (
        <View style={styles.toDo} key={key}>
          <Text style={styles.toDoText}>{toDos[key].text}</Text>
        </View>
      ) : null
   )}
</ScrollView>
```


데이터 저장하기 위해 localstorage와 같은 기능을 하는 AsyncStorage를 사용했습니다.<br>
saveToDos 함수로 key인 @toDos를 AsyncStorage에 저장해 주고 저장할 toDos를 stringily 해줬습니다.

```
const STORAGE_KEY = "@toDos";

const saveToDos = async (toSave) => {
  await AsyncStorage.setItem(STORAGE_KEY, JSON.stringify(toSave));
}; 
```

toSave 형태의 toDos를 받고 addToDo 함수를 통해서 saveToDos에 전해주도록 할거라 addToDo 함수 내에 작성해 줬습니다.<br>
addToDo 함수에서는 newToDos도 받도록 했습니다.
즉, addToDo 함수 안에서 새로운 오브젝트를 만들어서 setToDos(newToDos)를 통해 state에 넣어주고 또 그 새로운 오브젝트를 saveToDos(newToDos)를 통해서 saveToDos 함수에 보내게 됩니다.

```
const addToDo = () => {
  if (text === "") {
    return;
  }
  const newToDos = { ...toDos, [Date.now()]: { text, working } };
  setToDos(newToDos);
  saveToDos(newToDos)
  setText("");
};
```

새로고침해도 Todo가 남아있도록 loadToDos 함수를 만들었습니다.
마찬가지로 AsyncStorage을 사용해서 저장해줬습니다.

```
const loadToDos = async () => {
  const s = await AsyncStorage.getItem(STORAGE_KEY);
  setToDos(JSON.parse(s));
};

useEffect(() => {
  loadToDos();
}, []);
```

앱을 재실행하면 useEffect()를 통해 loadToDos 함수를 호출하고 loadToDos에서는 AsyncStorage를 이용해서 @toDos를 받아오게 됩니다.<br>
그렇게 받아온 toDo들을 parse해서 오브젝트로 만듭니다.<br>
→ setToDos(JSON.parse(s));
<p>
JSON.parse()는 string을 받아서 자바스크립트 오브젝트로 반환하고 state에 전달합니다.<br>
결론적으로 rerender가 이루어지고 UI가 업데이트됩니다.
<p>
Todo를 삭제하기 위해 deleteToDo 함수를 만들었습니다.
deleteToDo 함수에 key를 받고 newToDos에 기존 toDos를 넣어주고 key를 삭제하도록 했습니다.

```
const deleteToDo = async (key) => {
  const newToDos = { ...toDos };
  delete newToDos[key];
  setToDos(newToDos);
  saveToDos(newToDos);
}
```

Todo를 삭제 전에 정말 삭제할 건지 알람을 보내기 위해 Alert API를 사용했습니다.

```
Alert.alert("Delete To Do?", "Are you sure?", [
  { text: "Cancle" },
  {
    text: "I'm Sure",
    style: "destructive",
    onPress: () => {
      const newToDos = { ...toDos };
      delete newToDos[key];
      setToDos(newToDos);
      saveToDos(newToDos);
    },
  },
])
```

### 라이브러리 설치
```shell
npm install react-redux
```
### Slice 생성
```jsx
// /store/memo/Memo.slice.js
import {createSlice} from "@reduxjs/toolkit";  
  
const initialState = {  
  memos: [],  
}  
  
const memoSlice = createSlice({  
  name: "memo",  
  initialState,  
  reducers: {  
    setMemos: (state, action) => {  
      state.memos = action.payload;  
      localStorage.setItem("memos", JSON.stringify(action.payload));  
    },  
    addMemo: (state, action) => {  
      
    },  
    deleteMemo: (state, action) => {  
	  ... 
    },  
    moveMemoPosition: (state, action) => {  
      ...  
    }  
  }  
});  
  
export const memoActions = memoSlice.actions;  
export default memoSlice.reducer;
```

memoActions를 export 하여 외부에서 쉽게 memoSlice의 reducer 들을 dispatch할 수 있게 해준다.

```jsx
const store = configureStore ({  
  reducer: {  
    "memo": memoSlice  
  }  
})  
  
export default store;
```

redux store를 설정해줄 수 있다. 이후 useSelector를 통해 memo로 접근하면 memoSlice의 상태를 받아 사용할 수 있다.
```jsx
const memos = useSelector(state => state.memo.memos);
```

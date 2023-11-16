[[Masonry Layout]]
## React Masonry CSS 설치
```shell
npm install react-masonry-css
```

## Masonry Tag
```jsx
<Masonry  
  breakpointCols={breakpointColumnsObj}  
  className="my-masonry-grid"  
  columnClassName="my-masonry-grid_column"  
>  
  {memos.map((memo) => {  
    return (  
      <Memo 
	      key={memo.id} title={memo.title} 
	      content={memo.content} id={memo.id} 
      />  
    );  
  })}  
</Masonry>
```
Masonry라는 태그에 배치될 요소들을 태그 내부에 넣어주면 된다.
```jsx
const breakpointColumnsObj = {  
  default: 4,  
  1100: 3,  
  700: 2,  
  500: 1  
};
```
위와 같은 형식으로 페이지의 너비에 따라 Column의 개수를 조절할 수 있다.
```css
.my-masonry-grid {  
    display: -webkit-box; /* Not needed if autoprefixing */  
    display: -ms-flexbox; /* Not needed if autoprefixing */  
    display: flex;  
    width: 100%;  
    margin-left: 0; /* gutter size offset */  
}  
  
.my-masonry-grid_column {  
    padding-left: 0; /* gutter size */  
}  
  
/* Style your items */  
.my-masonry-grid_column > div { /* change div to reference your elements you put in <Masonry> */  
    margin-bottom: 30px;  
}
```
적당히 조절해야함
#masonry

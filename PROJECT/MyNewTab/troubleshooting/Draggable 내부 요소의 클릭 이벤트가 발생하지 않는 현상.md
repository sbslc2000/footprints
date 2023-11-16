# Issue
```jsx
...
return (  
      <YoutubeContext.Provider value = {youtubeContextValue}>  
        <DndContext onDragEnd={handleDragEnd} sensors={sensors}>  
          <DroppableWrapper>  
		        <YoutubeWidgetHandler x={widgetX} y={widgetY} 
		        isPlaying={isPlaying} url={youtubeUrl}/>  
			    {props.children}  
          </DroppableWrapper>  
        </DndContext>  
      </YoutubeContext.Provider>  
  );
```
위와 같이 DndContext를 사용하고 있고, YoutubeWidgetHandler 내부 깊은 곳에는 다음과 같은 컴포넌트가 있다.
```jsx
const WidgetHeader = () => {  
  const YoutubeState = useContext(YoutubeContext);  
  
  const onDeleteHandler = (event) => {  
    event.stopPropagation();  
    console.log("close");  
    YoutubeState.setIsPlaying(false);  
  }  
  
  return (  
    <Wrapper>      
	    <AiFillCloseCircle  
	        style={style}  
	        onClick={onDeleteHandler}  
	    />  
    </Wrapper>  
  );  
}
```
AiFillCloseCircle을 클릭하면 onDeleteHandler 이벤트가 실행되길 원했는데, DndContext의 onDragEnd 이벤트가 호출되고 클릭이벤트는 발생하지 않았다. 여러번 클릭을 하면 간헐적으로 내부 이벤트가 수행되긴하는데 이유를 찾지 못했다.
# Cause
내부의 이벤트가 간헐적으로 실행되는 것으로 봐서 이벤트 버블링과 관련된 문제는 아닌 것 같다. 
https://github.com/clauderic/dnd-kit/issues/355 
"이것은 버그가 아닙니다. DragOverlay를 사용할 때 onClick 이벤트가 발생하지 않는 것은 예상되는 동작입니다. 드래그 시작시 드래그 오버레이 컴포넌트가 정렬된 아이템 위에 렌더링되기 때문입니다.
이 문제를 해결하기 위한 몇 가지 방법이 있습니다. 드래그 이벤트를 활성화하기 전에 지연시키거나 특정 거리를 드래그할 때까지 기다리는 활성화 제약 조건을 추가할 수 있습니다. 또는 드래그 핸들을 사용할 수도 있습니다."
# Solution
[commit link](https://github.com/sbslc2000/mynewtab/commit/f1ccc5f36e8b3d511709f7d4a753d59a8c16140e)
```jsx
...
const sensor = useSensor(MouseSensor, {  
  activationConstraint: {   
    distance: 10,  //10px 이상 움직일 때만 드래그 이벤트 발생
  }  
});  
  
const sensors = useSensors(sensor);

...

return (
...
	<DndContext ... sensors={sensors} //센서 등록
	...
	/>
...
)
```

## More 


## Tag
#dnd-kit 

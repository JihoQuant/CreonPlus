# Creon Plus

> 대신증권에서 제공하는 Creon Plus API를 사용하기 쉽게 재구성한 패키지입니다.
> 
> jihogrammer@gmail.com


***현재 이 패키지는 개발 중입니다.***


## TODO

- `requirements.txt` 명시
- Windows에서 테스트 및 예제 개선
- `pywin32` 종속성과 충돌 없는지 파악
- Creon Plus 자동 로그인
- Creon Plus 장애 발생 시 복구 및 예외처리

---

## Example - 실시간 가격 구독

> 테스트 안 됨 - OSX에서 구현해서 Windows에서 확인해야 함

패키지 내 examples 폴더에서 확인할 수 있으며, 주석은 pydoc이 익숙해지면 추가하겠습니다.

현재 내용은 일종의 슈도코드(Pseudocode) 정도입니다.

```python
class MyHandler(LivePriceHandler):
    '''Creon Plus가 발생시키는 이벤트를 감지하는 함수 - 함수명은 반드시 "OnReceived"으로 고정'''
    def OnReceived(self) -> None:
        dto = LivePrice(
            self.__module.GetHeaderValue(0),
            self.__module.GetHeaderValue(1),
            self.__module.GetHeaderValue(13),
            self.__module.GetHeaderValue(18),
        )
        print(dto)


class Subscriber:
    __module: 'Module'

    def __init__(self):
        # Creon 모듈 생성
        self.__module = get_stock_cur_module()
        # 이벤트 등록
        self.__handler = bind_module_with_eventhandler(self.__module, MyHandler)
        # 핸들러에 모듈 등록
        self.__handler.init(self.__module)

    def subscribe(self):
        self.__handler.subscribe()


def example():
    '''실행 예시 - 실제로는 Creon 로그인과 pythoncom 핸들링이 필요함'''
    Subscriber().subscribe()
```


```python
# creon-plus/__init__.py

if __name__ == '__main__':
    import examples
    examples.example_live_price_subscribe()
```

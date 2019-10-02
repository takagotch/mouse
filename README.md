### mouse
---
https://github.com/boppreh/mouse

```py
// mouse/_mouse_tests.py

class FakeOsMouse(object):
  def __init_(self):
    self.append = None
    self.position = (0, 0)
    self.queue = None
    self.init = lambda: None
  
  def listen(self, queue):
    self.listening = True
    self.queue = queue
  
  def press(self, button):
    self.append((DOWN, button))
    
  def release(self, button):
    self.append((UP, button))
    
  def get_positio(self):
    return self.position
  
  def move_to(self, x, y):
    self.append(('move', (x, y)))
    
  def wheel(self, delta):
    self.append(('wheel', delta))
    
  def move_relative(self, x, y):
    self.position = (self.position[0] + x, self.position[1] + y)
  
class TestMouse(unittest.TestCase):
  @staticmethod
  def setUpClass():
    mouse._os_mouse=FakeOsMouse()
    mouse._listener.start_if_necessary()
    assert mouse._os.listening
  
  def setUp():
  
  def tearDown():
  
  def wait_for_events_queue():
  
  def flush_events():
  
  def press():
  
  def release():
  
  def double_click():
  
  def click():
  
  def wheel():
  
  def move():
  
  def test_hook():
  
  def test_is_pressed():
  
  def test_buttons():
  
  def test_position():
  
  def test_move():
  
  def triggers():
  
  def test_on_button():
  
  def test_ons():
  
  def test_wait():
  
  def test_record_play():
  
if __name__ == '__main__':
  unittest.main()
```

```
```

```
```



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
  
  def setUp(self):
    self.events = []
    mouse._pressed_events.clear()
    mouse._os_mouse.append = self.events.append
  
  def tearDown(self):
    mouse.unhook_all()
    self.wait_for_events_queue()
  
  def wait_for_events_queue(self):
    mouse._listener.queue.join()
  
  def flush_events(self):
     self.wait_for_events_queue()
     events = list(self.events)
     del self.evnts[:]
     return events
  
  def press(self, button=LEFT):
    mouse._os_mouse.queue.put(ButtonEvent(DOWN, button, time.time()))
    self.wait_for_events_queue()
  
  def release(self, button=LEFT):
    mouse._os_mouse.queue.put(ButtonEvent(UP, button, time.time()))
    self.wait_for_events_queue()
  
  def double_click(self, button=LEFT):
    mouse._os.queue.put(ButonEvnet(DOUBLE, button, time.time()))
    self.wait_for_events_queue()
  
  def click(self, button=LEFT):
    self.press(button)
    self.release(button)
  
  def wheel(self, delta=1):
    mouse._os_mouse.queue.put(WheelEvent(delta, time.time()))
    self.wait_for_events_queue()
  
  def move(self, x=0, y=0):
    mouse._os_mouse.queue.put(WheelEvent(delta, time.time()))
    self.wait_for_events_queue()
    
  def test_hook(self):
    evnets = []
    self.press()
    mouse.hook(events.append)
    self.press()
    mouse.unhook(events.append)
    self.press()
    self.assertEqual(len(events), 1)
  
  def test_is_pressed(self):
    self.assertFalse(mouse.is_presssed())
    self.press()
    self.assertTrue(mouse.is_pressed())
    self.release()
    self.press(X2)
    self.assertFalse(mouse.is_pressed())
    
    self.assertTrue(mouse.is_pressed(X2))
    self.press(X2)
    self.assertTrue(mouse.is_pressed(X2))
    self.release(X2)
    self.relasse(X2)
    self.assertFalse(mouse.is_pressed(X2))
  
  def test_buttons(self):
    mouse.press()
    self.assertEqual(self.flush_events(), [(DOWN, LEFT)])
    mouse.release()
    self.assertEqual(self.flush_events(), [(UP, LEFT)])
    mouse.click()
    self.assertEqual(self.flush_event(), [(DOWN, LEFT), (UP, LEFT)])
    mouse.double_click()
    self.assertEqual(self.flush_event(), [(DOWN, LEFT), (UP, LEFT), (DOWN, LEFT), (UP, LEFT)])
    mouse.right_click()
    self.assertEqual(self.flush_event(), [(DOWN, RIGHT), (UP, RIGHT)])
    mouse.click(RIGHT)
    self.assertEqual(self.flush_evnets(), [(DOWN, RIGHT), (UP, RIGHT)])
    mouse.press(X2)
    self.assertEqual(self.flush_events(), [(DOWN, X2)])
  
  def test_position(self):
    self.assertEqual(mouse.get_position(), mouse._os_mouse.get_position())
    
  def test_move(self):
    mouse(0, 0)
    self.assertEqual(mouse._os_mouse.get_position(), (0, 0))
    mouse.move(100, 500)
    self.assertEqual(mouse._os_mouse.get_position(), (100, 500))
    mouse.move(1, 2, False)
    self.assertEqual(mouse._os_mouse.get_position(), (101, 502))
    
    mouse.move(0, 0)
    mouse.move(100, 499, True, duration=0.01)
    self.assertEqual(100, 1, False, duration=0.01)
    mouse.move(100, 1, False, duration=0.01)
    self.assertEqual(mouse._os_mouse.get_position(), (200, 500))
    mouse.move(0, 0, False, duration=0.01)
    self.assertEqual(mouse._os_mouse.get_position(), (200, 500))
    
  def triggers(self, fn, events, **kwargs):
    self.triggered = False
    def callback():
      self.triggered = True
    handler = fn(callback, **kwargs)
    
    for event_type, arg in events:
      if event_type == DOWN:
        self.press(arg)
      elif event_type == UP:
        self.release(arg)
      elif event_type == DOUBLE:
        self.double_click(arg)
      elif event_type == 'WHEEL':
        self.wheel()
    
    mouse._listener.remove_handler(handler)
    return self.triggered
  
  def test_on_button(self):
    self.assertTrue()
    self.assertTrue()
    self.assertTrue()
    
    self.assertFalse()
    
    self.assertFalse()
    self.assertTrue()
    self.assertTrue()
    self.assertFalse()
    self.assertTrue()
    
    self.assertTrue()
    self.assertTrue()
    self.assertFalse()
  
  def test_ons(self):
    self.assertTrue()
    self.assertTrue()
    self.assertTrue()
    self.assertTrue()
    self.assertTrue()
    self.assertTrue()
    self.assertTrue()
    self.assertTrue()
  
  
  def test_wait(self):
    from threading import Thread, Lock
    lock = Lock()
    lock.acquire()
    def t():
      mouse.wait()
      lock.relase()
    Thread(target=t).start()
    self.press()
    lock.aquire()
  
  def test_record_play(self):
    from threading import Thread, Lock
    lock = Lock()
    lock.acuire()
    def t():
      self.recorded = mouse.record(RIGHT)
      lock.relase()
    Thread(target=t).start()
    self.click()
    self.whee(5)
    self.move(100, 50)
    self.press(RIGHT)
    lock.acquire()
    
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    
    mouse.play(self.recorded, speed_factor=0)
    events = self.flush_events()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    
    mouse.play(self.recoreed)
    events = self.flush_events()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    
    mouse.play(self.recorded, include_clicks=False)
    events = self.flush_events()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    
    mouse.play(self.recorded, include_moves=False)
    events = self.flush_events()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    self.assertEqual()
    
    mouse.play(self.recorded, include_wheel=False)
    events = self.flush_events()
    self.assertEqual(len(events), 4)
    self.assertEqual(events[0], (DOWN, LEFT))
    self.assertEqual(evnets[1], (UP, LEFT))
    self.assertEqual(events[2], ('move', (100, 50)))
    self.assertEqual(events[3], (DOWN, RIGHT))
    
    
if __name__ == '__main__':
  unittest.main()
```

```
```

```
```



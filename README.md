from kivy.config import Config 
Config.set('graphics', 'resizable', 1)
Config.set('graphics', 'height', 810)
Config.set('graphics', 'width', 400)
#импорт приложения, кнопок, лайаутов, надписей и т.д
from kivy.app import App 
from kivy.uix.button import Button
from kivy.uix.label import Label 
from kivy.uix.image import Image
from kivy.uix.widget import Widget
from kivy.animation import Animation
from kivy.uix.screenmanager import Screen, ScreenManager
from kivy.uix.textinput import TextInput
from kivy.uix.boxlayout import BoxLayout 
from kivy.core.window import Window
from kivy.uix.anchorlayout import AnchorLayout
from kivy.uix.gridlayout import GridLayout
from kivy.properties import StringProperty, NumericProperty
#заготовка надписей
txt_instruction = '''
Данное приложение позволит \nвам с помощью теста Руфье \n провести первичную диагностику \nвашего здоровья.\n
Проба Руфье представляет собой \nнагрузочный комплекс, \nпредназначенный для \nоценки работоспособности \nсердца при физической нагрузке.
\n'''
txt_test1 = '''Измерьте кол-во сердцебиений за 15 секунд, \n\nв положении лежа последние 2 минуты\n'''
txt_test2 = '''Выполните 30 приседаний за 45 секунд.\n 
Нажмите кнопку "Приступить к выполнению", чтобы запустить счетчик приседаний.\n
Делайте приседания со скоростью счетчика.'''
txt_test3 = '''В течение минуты замерьте кол-во сердцебиений два раза:\n 
За первые 15 секунд минуты, затем за последние 15 секунд.\n
Результаты запишите в соответствующие поля.'''
txt_sits = 'Выполните 30 приседаний за 60 секунд.'
#создание экрана №1
class InfoScreen(Screen):
    #описание класса
    def __init__(self):
        super().__init__()
        self.name = 'infoscr'
        Window.clearcolor = 'black'
        al = AnchorLayout()
        bl = BoxLayout(orientation='vertical', padding=0, spacing=2, size_hint=[1, 1])
        self.add_widget( al)
        self.img1 = Image(source='ruffie1.png', size_hint=[1, 10])
        bl.add_widget( self.img1)
        self.labl = Label(text=txt_instruction, text_size=[700, 390], font_size=22, 
        size_hint=[1.02, 13], valign='top', halign='center')
        bl.add_widget( self.labl)
        bl.add_widget( Widget())
        box = BoxLayout(spacing=-2, size_hint=[1,1])
        box.add_widget( Label(text='Введите имя:', font_size=22, size_hint=[0.7, 1.55], text_size=[140, None]))
        self.nametxt = TextInput(size_hint=[0.9, 1.5], multiline=False, hint_text='Введите ваше имя:')
        box.add_widget( self.nametxt)
        bl.add_widget( box)
        bl.add_widget( Widget())
        bx = BoxLayout(spacing=-2, size_hint=[1,1])
        bx.add_widget( Label(text='Введите возраст:', font_size=19, size_hint=[0.7, 1.55]))
        self.agetxt = TextInput(size_hint=[0.9, 1.5], multiline=False, 
        input_filter='int', hint_text='Введите ваш возраст:')
        bx.add_widget( self.agetxt)
        bl.add_widget( bx)
        for i in range( 4):
            bl.add_widget( Widget())
        boxl = BoxLayout(size_hint=[1,1])
        boxl.add_widget( Widget())
        boxl.add_widget( Button(text='Начать', background_color='lime',
        font_size=30, size_hint=[1.1, 5], pos_hint={'center_y' : 0.5},
        on_press = self.check))
        boxl.add_widget( Widget())
        bl.add_widget( boxl)
        al.add_widget( bl)
        for i in range( 3):
            bl.add_widget( Widget())
    #функция проверки ввода
    def check(self, instance):
        if self.nametxt.text == '':
            self.labl.text += 'Сначала введите все данные!'
        elif self.agetxt.text == '':
            self.labl.text += 'Сначала введите все данные!'
        else:
            self.next( instance)
    #функция переключения экранов
    def next(self, instance):
        self.manager.transition.direction = 'left'
        self.manager.current = 'test1scr'
        
#создания экрага №2
class Test1Screen(Screen):
    #описание класса
    def __init__(self):
        super().__init__()
        self.name = 'test1scr'
        bl = BoxLayout(orientation='vertical', padding=5, spacing=2)
        self.add_widget( bl)
        bl.add_widget( Label(text=txt_test1+'\nЗапомните результат\nОн вам пригодиться в дальнейшем.', text_size=[700, 270], font_size=30, 
        size_hint=[1.02, 18], valign='bottom', halign='center'))
        box = BoxLayout()
        for i in range( 3):
            bl.add_widget( Widget())
        bx = BoxLayout()
        bx.add_widget( Widget())
        bx.add_widget( Button(text='Назад', background_color='green',
        font_size=30, size_hint=[0.9, 5], pos_hint={'center_y' : 0.5}, on_press = self.back))
        bx.add_widget( Widget())
        bx.add_widget( Button(text='Продолжить', background_color='green',
        font_size=30, size_hint=[1.45, 5], pos_hint={'center_y' : 0.5},
        on_press = self.next))
        bx.add_widget( Widget())
        bl.add_widget( bx)
        for i in range( 3):
            bl.add_widget( Widget())
    #функция переключения экранов
    def next(self, *args):
        self.manager.transition.direction = 'left'
        self.manager.current = 'test2scr'
    #функция возврата на прошлый экран
    def back(self, *args):
        self.manager.transition.direction = 'right'
        self.manager.current = 'infoscr'
#создание экрана №3
class Test2Screen(Screen):
    #описание класса
    def __init__(self):
        super().__init__()
        self.name = 'test2scr'
        bl = BoxLayout(orientation='vertical', padding=5, spacing=2)
        self.add_widget( bl)
        bl.add_widget( Label(text=txt_test2, text_size=[700, 300], font_size=25, 
        size_hint=[1.02, 19], valign='center', halign='center'))
        box = BoxLayout()
        box.add_widget( Widget())
        box.add_widget( Button(text='Назад', background_color='green',
        font_size=30, size_hint=[1.05, 5], pos_hint={'center_y' : 0.5},
        on_press = self.back))
        box.add_widget( Widget())
        box.add_widget( Button(text='   Приступить \nк выполнению', background_color='green',
        font_size=30, size_hint=[1.85, 5], pos_hint={'center_y' : 0.5},
        on_press = self.next))
        box.add_widget( Widget())
        bl.add_widget( box)
        for i in range( 4):
            bl.add_widget( Widget())
    #функция переключения экранов
    def next(self, *args):
        self.manager.transition.direction = 'left'
        self.manager.current = 'test3scr'
    #функция возврата на прошлый экран
    def back(self, *args):
        self.manager.transition.direction = 'right'
        self.manager.current = 'test1scr'
#создание экрана №4
class Test3Screen(Screen):
    #описание класса
    def __init__(self):
        super().__init__()
        self.name = 'test3scr'
        bl = BoxLayout(orientation='vertical', padding=5, spacing=2)
        self.add_widget( bl)
        self.lbl1 = Label(text='Выполняйте приседания со \nскоростью счетчика.', 
        text_size=[700, 300], font_size=25, 
        size_hint=[1.02, 19], valign='center', halign='center')
        bl.add_widget( self.lbl1)
        self.btn1 = Button(text='Приступить', background_color='green',
        font_size=30, size_hint=[3.45, 5], pos_hint={'center_y' : 0.5}, on_press = self.animation)
        box = BoxLayout()
        box.add_widget( Widget())
        self.btn4 = Button(text='Назад', background_color='green',
        font_size=30, size_hint=[1.9, 5], pos_hint={'center_y' : 0.5}, on_press = self.back1)
        box.add_widget( self.btn4)
        self.lbl2 = Label(text='', text_size=[700, 300], font_size=25, 
        size_hint=[1.02, 19], valign='center', halign='center')
        box.add_widget( self.lbl2)
        self.btn3 = Button(text=' ', background_color='black', 
        font_size=30, size_hint=[2.4, 5], pos_hint={'center_y' : 0.5})
        box.add_widget( self.btn3)
        box.add_widget( Widget())
        box.add_widget( self.btn1)
        box.add_widget( Widget())
        self.btn2 = Button(background_color='black', size_hint=[1.65,5], font_size=15)
        box.add_widget( Widget())
        box.add_widget( self.btn2)
        bl.add_widget( box)
        for i in range( 3):
            bl.add_widget( Widget())
    #функция создания анимации
    def animation(self, *args):
        self.btn4.on_press = self.back2
        self.btn3.on_press = self.next
        self.lbl1.text = 'Выполняйте приседания со \nскоростью счетчика.\n\nВыполняйте приседания в\n этом темпе -->\n\n"В этом цикле 30 повторений \n1 повторение - 2 секунды."'
        self.btn3.background_color = 'green'
        self.btn3.text = 'Далее'
        self.btn1.text = ' '
        self.btn1.background_color = 'black'
        self.btn2.background_color = 'blue'
        self.anim = Animation(y=400) 
        for i in range( 29):
            self.anim+=Animation(y=10)
            self.anim += Animation(y=410)
        self.anim+=Animation(y=10)
        self.anim.start( self.btn2)
    #функция повторного запуска анимации
    def again(self, *args):
        self.anim = Animation(y=400)
        for i in range(29):
            self.anim+=Animation(y=10)
            self.anim += Animation(y=410)
        self.anim+=Animation(y=10)
        self.anim.start( self.btn2)
    #функция переключения экранов
    def next(self, *args):
        self.btn1.on_press = self.again
        self.btn1.size_hint = [3.95, 5]
        self.btn1.text = 'Начать\nснова'
        self.btn1.background_color = 'green'
        self.anim.stop( self.btn2)
        self.manager.transition.direction = 'left'
        self.manager.current = 'finalscr'
    #функция возврата на прошлый экран
    def back1(self, *args):
        self.manager.transition.direction = 'right'
        self.manager.current = 'test2scr'
    #функция возврата на прошлый экран с остановкой анимации
    def back2(self, *args):
        self.btn1.on_press = self.again
        self.btn1.size_hint = [3.95, 5]
        self.btn1.text = 'Начать\nснова'
        self.btn1.background_color = 'green'
        self.anim.stop( self.btn2)
        self.manager.transition.direction = 'right'
        self.manager.current = 'test2scr'
#создание экрана №5
class FinalScreen(Screen):
    #описание класса
    def __init__(self):
        super().__init__()
        self.name = 'finalscr'
        bl = BoxLayout(orientation='vertical')
        self.add_widget(bl)
        self.resultlbl = Label(text=txt_test3+'\nЗапишите результаты\n№1 - после 1-ого измерения\n№2 - после второго\n№3 - после последнего.\nНажмите на кнопку, чтобы узнать результат.',
        text_size=[700, 300], font_size=22, 
        size_hint=[1.02, 19], valign='center', halign='center')
        bl.add_widget( self.resultlbl)
        self.txt1 = TextInput(input_filter='int', size_hint=[0.85, 2], hint_text='Ваш результат №1:')
        self.txt2 = TextInput(input_filter='int', size_hint=[0.85, 2], hint_text='Ваш результат №2:')
        self.txt3 = TextInput(input_filter='int', size_hint=[0.85, 2], hint_text='Ваш результат №3:')
        box = BoxLayout()
        box.add_widget( Label(text='Введите результат #1', font_size=25, size_hint=[0.7, 1.85], text_size=[None, None]))
        box.add_widget( self.txt1)
        bx = BoxLayout()
        bx.add_widget( Label(text='Введите результат #2', font_size=25, size_hint=[0.7, 1.85], text_size=[None, None]))
        bx.add_widget( self.txt2)
        bxl = BoxLayout()
        bxl.add_widget( Label(text='Введите результат #3', font_size=25, size_hint=[0.7, 1.85], text_size=[None, None]))
        bxl.add_widget( self.txt3)
        bl.add_widget( box)
        bl.add_widget( Widget())
        bl.add_widget( Widget())
        bl.add_widget( bx)
        bl.add_widget( Widget())
        bl.add_widget( Widget())
        bl.add_widget( bxl)
        for i in range( 5):
            bl.add_widget( Widget())
        boxl = BoxLayout()
        boxl.add_widget( Widget())
        boxl.add_widget( Button(text='Назад', background_color='green',
        font_size=30, size_hint=[1.35, 5], pos_hint={'center_y' : 0.5},
        on_press = self.back))
        boxl.add_widget( Widget())
        boxl.add_widget( Button(text='   Узнать\nрезультаты', background_color='green',
        font_size=30, size_hint=[2.6, 5], pos_hint={'center_y' : 0.5},
        on_press = self.check))
        boxl.add_widget( Widget())
        self.btn6 = Button(text='', background_color='black',
        font_size=30, size_hint=[2.6, 5], pos_hint={'center_y' : 0.5})
        boxl.add_widget( self.btn6)
        boxl.add_widget( Widget())
        bl.add_widget(boxl)
        for i in range(3):
            bl.add_widget( Widget())
    #функция возврата на прошлый экран
    def back(self, *args):
        self.manager.transition.direction = 'right'
        self.manager.current = 'test3scr'
    #функция переключения на последний экран
    def end(self, *args):
        self.manager.transition.direction = 'left'
        self.manager.current = 'endscr'
    #функция проверки ввода
    def check(self, instance):
        if self.txt1.text == '':
            self.resultlbl.text += '\nСначала введите все результаты!'
        elif self.txt2.text == '':
            self.resultlbl.text += '\nСначала введите все результаты!'
        elif self.txt3.text == '':
            self.resultlbl.text += '\nСначала введите все результаты!'
        else:
            self.count_result(instance)
    #функция подсчета индекса Руфье
    def count_result(self, instance):
        self.formula = '('+self.txt1.text+'*4'+'+'+self.txt2.text+'*4'+'+'+self.txt3.text+'*4-200)/10'
        self.result_str = str(eval(self.formula))
        self.result_int = float(self.result_str)
        self.result1 = '\nЭто отличный результат, которого \nмогут добиться немногие спортсмены.'
        self.result2 = '\nЭто хороший результат, он означает, что вы \nздоровый человек в хорошей физ. форме.'
        self.result3 = '\nЭто нормальный результат. Вы в удовлетворительной \nфиз. форме, причин для беспокойствия нету.'
        self.result4 = '\nЭто значит, что вы в плохой физ. форме. Вам стоит \nпобеспокоиться о своем организме'
        if self.result_int <= 0 and self.result_int >-12:
            self.resultlbl.text = 'Ваш результат: '+self.result_str+self.result1
            self.btn6.text = 'Завершить\nпрограмму'
            self.btn6.background_color = 'green'
            self.btn6.on_press = self.end
        elif self.result_int <= 5 and self.result_int >0:
            self.resultlbl.text = 'Ваш результат: '+self.result_str+self.result2
            self.btn6.text = 'Завершить\nпрограмму'
            self.btn6.background_color = 'green'
            self.btn6.on_press = self.end
        elif self.result_int <= 10 and self.result_int >5:
            self.resultlbl.text = 'Ваш результат: '+self.result_str+self.result3
            self.btn6.text = 'Завершить\nпрограмму'
            self.btn6.background_color = 'green'
            self.btn6.on_press = self.end
        elif self.result_int <= 40 and self.result_int >10:
            self.resultlbl.text = 'Ваш результат: '+self.result_str+self.result4 
            self.btn6.text = 'Завершить\nпрограмму'
            self.btn6.background_color = 'green'
            self.btn6.on_press = self.end
        else:
            self.resultlbl.text = 'Скорее всего вы ввели неправильные данные\nПопробуйте снова.'
#создание последнего экрана №6
class EndScreen(Screen):
    #описание класса
    def __init__(self):
        super().__init__()
        self.name = 'endscr'
        bl = BoxLayout()
        self.add_widget( bl)
        self.endlbl = Label(text='Спасибо, что воспользовались\n данной программой!', text_size=[700, 300], font_size=40, 
        size_hint=[1.02, 19], valign='center', halign='center')
#создание класса приложения
class Test_RuffieApp(App):
    #функция построения приложения из экранов
    def build(self):
        sm = ScreenManager()
        sm.add_widget( InfoScreen())
        sm.add_widget( Test1Screen())
        sm.add_widget( Test2Screen())
        sm.add_widget( Test3Screen())
        sm.add_widget( FinalScreen())
        sm.add_widget( EndScreen())
        return sm
#запуск приложения
app = Test_RuffieApp()
app.run()

Простая инструкция как включить гибридную графику intel-nvidia на ноутбуке
1. Устанавливаем программу для загрузки драйверов:
        sudo apt install software-properties-qt
2. Запускаем с помощью
        sudo software-properties-qt
        или вместо qt gtk

 также можно добавить ее в панель приложений (для KDE), для этого я создал два файла `software properties qt.desktop` и `software properties qt.sh` (поместить в `~/.local/share/applications/`)
3. Переходим на последнюю вкладку `Additional drivers`и устанавливаем нужный драйвер. Я выбрал самый последний, который не `tested` и не `server`
4. Перезагружаем ноутбук
5. Устанавливаем
        sudo apt install nvidia-settings
 запускаем эту программу.
6. Переходим в `PRIME Profiles`

  a) `NVIDIA (Performance Mode)` - работать только на дискретной видеокарте. Сильно потребляет батарею в несложных задачах, зато намного быстрее

  b) `NVIDIA On-Demand` - некоторые приложения будут использовать дискретную графику nvidia, но по-умолчанию встроенная intel. Как запустить приложение с дискретной напишу дальше

  c) `NVIDIA (Power Saving Mode)` - отключение дискретной графики полностью
7. Выбираем второй вариант - `NVIDIA On-Demand`
8. Перезагружаем ноутбук
9. Для запуска приложения с использованием видеокарты nvidia нужно задать для `OpenGL` две переменные среды:
        __NV_PRIME_RENDER_OFFLOAD=1
        __GLX_VENDOR_LIBRARY_NAME=nvidia
 для `Vulkan` только
        __NV_PRIME_RENDER_OFFLOAD=1
10. Покажу как сделать это на примере конкретного приложения - игра `Wolfenstein - Blade of Agony`

  a) сначала у нас должен быть .desktop файл приложения. После установки у меня он уже есть в менб приложений. Заходим в его свойства (ПКМ - изменить приложение)

  b) переходим во вкладку "приложение", находим комманду для запуска и перед ней приписываем `__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia`

        Так, что было:
        env BAMF_DESKTOP_FILE_HINT=/var/lib/snapd/desktop/applications/boa_boa.desktop /snap/bin/boa

        Стало:
        __NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia env BAMF_DESKTOP_FILE_HINT=/var/lib/snapd/desktop/applications/boa_boa.desktop /snap/bin/boa

  Eсли не совсем понятно, так как сложная комманда для запуска этой игры, то объясню на очень простом (но бесполезном) примере:

  Чтобы запустить программу `program` с дискретной видеокартой нужно написать
        __NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia program

  b) Можно сделать тоже самое через текстовый редактор: открываем `.desktop` файл, находим `Exec=` и перед коммандой приписываем эти две переменные среды
11. С `Windows` приложениями через `wine` почему-то не работает

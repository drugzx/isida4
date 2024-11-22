# isida4
Installation - xnamed/isida-ar GitHub Wiki
التنصيب
المتطلبات
كل شيء بسيط إلى حد كبير.

البرامج التالية مطلوبة لتنصيب isida:

python >= 2.7 < 3.x
git-core >= 1.7.1 or subversion* >= 1.6.12
postgresql >= 8.4 or mysql* >= 5.1
openssl >= 1.0.0e
python-psycopg2 >= 2.4.2 or mysqldb-python* (libpq-dev و python-dev قد يكون مطلوبًا أيضًا)
python-crontab >= 0.12
pysqlite3 من توزيعة البايثون الحالية
لا ينصح بهذا الخيار

مثال على كيفية بناء و تنصيب البايثون:

... # build and install the dependencies first
wget http://python.org/ftp/python/2.7.3/Python-2.7.3.tar.bz2
tar xjf Python-2.7.3.tar.bz2
cd Python-2.7.3
./configure && make -j4 && sudo checkinstall
قبل محاولة بناء البايثون يجب عليك تنصيب تبعياته

تنصيب البوت
git clone git://github.com/xnamed/isida4.git isida
إعدادات البوت
في مجلد isida/settings اعد تسمية الملف demo_config.py إلى config.py وعدله:

Settings = {
'nickname': 	u'<bot nickname goes here>',			# لقب البوت
'jid':		u'isida-jabber-bot@domain.tld/isida-bot',	# حساب البوت
'password':	u'********',					# كلمه السر
'status':	u'online',					# حالة البوت chat|online|away|xa|dnd
'priority':	0,						# الاولوية
'message':	u'I am an infernal piece of software'}		# رسالة الحالة

SuperAdmin	= u'aaa@bbb.ru'					# حساب مالك البوت
defaultConf	= u'room@conference.server.tld'			# الغرفة
prefix		= u'_'						# بادئة لأوامر
msg_limit	= 2048						# حد طول الرسالة
base_type	= 'pgsql'	# نوع قاعدة البيانات: pgsql, mysql, sqlite3
base_name	= 'isidabot'	# اسم قاعدة البيانات
base_user	= 'isidabot'	# مستخدم قاعدة البيانات
base_host	= 'localhost'	# مضيف قاعدة البيانات
base_pass	= '******'	# كلمه سر قاعدة البيانات
base_port	= '5432'	# منفذ الاتصال. الافتراضي لـ postgresql 5432, لـ mysql 3306
تحذير! تتطلب بعض إصدارات mysqldb-python تعيين المنفذ كقيمة عددية (أي بدون علامات اقتباس). مثال:

base_port	= 3306
تحذير! تتوفر العديد من إعدادات البوت عبر أوامر ad-hoc أو عبر اكتشاف الخدمات.

الإجراءات اللازمة لتشغيل البوت
يجب عليك اتباع هذه الخطوات لإعداد البوت:

إنشاء قاعدة البيانات في PostgreSQL أو MySQL أو SQLite3 ؛
استيراد البيانات ونسخها الى PostgreSQL أو MySQL أو SQLite3.
الإجراءات لـ PostgreSQL:
su postgres						# تغيير المستخدم الى postgres
createuser -P isidabot					# إنشاء مستخدم مع كلمة مرور
createdb isidabot -E UTF8 -T template0			# إنشاء قاعدة بيانات تحتوي على ترميز UTF8 من نموذج template0
psql -U isidabot isidabot -f scripts/pgsql.schema	# إنشاء الجداول
psql -U isidabot isidabot -f data/db/defcodes.dump	# استيراد البيانات ونسخها
psql -U isidabot isidabot -f data/db/dist.dump
psql -U isidabot isidabot -f data/db/gis.dump
psql -U isidabot isidabot -f data/db/wz.dump
exit							# العودة إلى استخدام البوت
هام! بعد إنشاء قاعدة البيانات واستيراد عمليات التفريغ ، يُنصح بتنظيف قاعدة البيانات من التكرارات باستخدام الأمر التالي:

psql -U isidabot isidabot -f pgsql_remove_duplicates.schema
هام! إذا كنت تحصل على خطأ في المصادقة عند محاولة الاتصال بـ CheckgreSQL راجع صفحة التعليمات

الإجراءات لـ SQLite3:
cat scripts/sqlite3.schema | sqlite3 data/sqlite3.db	# إنشاء الجداول
cat data/db/defcodes.dump | sqlite3 data/sqlite3.db	# استيراد البيانات ونسخها
cat data/db/dist.dump | sqlite3 data/sqlite3.db
cat data/db/gis.dump | sqlite3 data/sqlite3.db
cat data/db/wz.dump | sqlite3 data/sqlite3.db
الإجراءات لـ MySQL:
mysql -u isidabot
SOURCE scripts/mysql.schema	# إنشاء الجداول
SOURCE data/db/defcodes.dump	# استيراد البيانات ونسخها
SOURCE data/db/dist.dump
SOURCE data/db/gis.dump
SOURCE data/db/wz.dump
تشغيل البوت
cd isida
sh launch.sh &
تحذير! لاحظ الرمز & في أمر التشغيل

استكشاف الأخطاء 

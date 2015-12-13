# ��װ
installation_centos7.sh

�����ؼ���
exchange
queue
routing_key
type
    fanout
    direct


# ����1 - hello world
python producer.py
python consumer.py


# ����2 - work queues
����ʱ����Ϣ����ͨ�����з�������consumer���������ǳƴ˴���consumerΪworker�����ǽ��˴���queue��ΪTask Queue����Ŀ����Ϊ�˱�����Դ�ܼ��͵�task��ͬ������Ҳ����������task���ȴ���ɡ��෴������taskʹ���Ժ󱻴���Ҳ����task��װ��message�����͵�task queue��worker�����ں�̨���У���task queueȡ��task��ִ��job���������˶��worker����task���ڶ��worker����䡣
task.py
�������ӣ��������У����Ϳ���ģ���ʱ�����message���Ͽ����ӡ��˳���
worker.py
�������ӣ��������У����ϵĽ���message���������񣬽���ȷ�ϡ�
python task.py "A very hard task which takes two seconds.."
python worker.py


Ӧ�ó���3 - Publish/Subscribe
��Ӧ�ó���2��һ��message(task)�������ݸ���һ��comsumer(worker)��
���������跨��һ��message���ݸ����consumer������ģʽ����Ϊpublish/subscribe��
�˴���һ���򵥵���־ϵͳΪ������˵������ϵͳ����һ��log���ͳ����һ��log���ղ���ӡ�ĳ�����log�����߷��͵�queue����Ϣ���Ա��������е�log�����߽��ա�
��ˣ����ǿ�������һ��log������ֱ������Ļ����ʾlog��ͬʱ������һ��log�����߽�logд������ļ���
receive_logs.py
��־��Ϣ�����ߣ��������ӣ�����exchange����exchange��queue���а󶨣���ʼ��ͣ�Ľ���log����ӡ��
emit_log.py
��־��Ϣ�����ߣ��������ӣ�����fanout���͵�exchange��ͨ��exchage��queue������־��Ϣ����Ϣ���㲥�����н����ߣ��ر����ӣ��˳���
python receive_logs.py
python emit_log.py "info: This is the log message"


Ӧ�ó���4 - Routing
Ӧ�ó���3�й����˼򵥵�logϵͳ�����Խ�log message�㲥�����receiver���������ǽ�����ֻ��ָ����message���ͷ��͸���subscriber�����磬ֻ��error messageд��log file��������log message��ʾ�ڿ���̨��
receive_logs_direct.py
log message�����ߣ��������ӣ�����direct���͵�exchange������queue��ʹ���ṩ�Ĳ�����Ϊrouting_key��queue�󶨵�exchange����ʼѭ������log message����ӡ��
emit_log_direct.py
log message�����ߣ��������ӣ�����direct���͵�exchange�����ɲ�����log message��exchange���ر����ӣ��˳���
python receive_logs_direct.py info
python emit_log_direct.py info "The message"


Ӧ�ó���5 - topic
Ӧ�ó���4�иĽ���logϵͳ����direct���͵�exchange�滻Ӧ�ó���3�е�fanout����exchangeʵ�ֽ���ͬ��log message���͸���ͬ��subscriber��Ҳ���ֱ�ͨ����ͬ��routing_key��queue�󶨵�exchange������exchange��ɽ���ͬ��message����message����·������ͬ��queue����
����Ȼ�������ƣ����ܸ��ݶ������·����Ϣ�����������Ҫôֻ����error���͵�log messageҪôֻ����info���͵�message��
������ǲ��������log����Ҫ������info��warning��error��������log message·�ɻ���ͬʱ����log message����Դ��auth��cron��kern������·�ɡ�Ϊ�˴ﵽ��Ŀ�ģ���Ҫtopic���͵�exchange��topic���͵�exchange��routing_key�п��԰������������ַ�����*���������һ���ʣ���#������0�������ʡ�
receive_logs_topic.py
log message�����ߣ��������ӣ�����topic���͵�exchange,����queue�����ݳ����������routing_key������routing_key��queue�󶨵�exchange��ѭ�����ղ�����message��
emit_log_topic.py
log message�����ߣ��������ӡ�����topic���͵�exchange�����ݳ����������routing_key��Ҫ���͵�message���Թ�����routing_key��message���͸�topic���͵�exchange���ر����ӣ��˳���
python receive_logs_topic.py "*.rabbit"
python emit_log_topic.py red.rabbit Hello


Ӧ�ó���6 - PRC
��Ӧ�ó���2�����������ʹ��work queue����ʱ��task���䵽��ͬ��worker�С����ǣ��������task������Զ�̵ļ����������һ���������ȴ����ؽ���ء��������2�е�������һ����ȫ��ͬ�Ĺ��¡���һģʽ����ΪԶ�̹��̵��á����ڣ����ǽ�����һ��RPCϵͳ������һ��client�Ϳ���չ��RPC server��ͨ������쳲���������ģ��RPC service��
rpc_server.py
RPC server���������ӣ�����queue��������һ������ָ�����ֵ�쳲��������ĺ�����������һ���ص������ڽ��յ����������ĵ������������Լ��ķ���쳲��������ĺ�������������͵�����յ�message��queue�������queue��������ȷ�ϡ���ʼ���յ��������ûص���������������
rpc_client.py
RPC client��Զ�̹��̵��÷����ߣ�������һ���࣬���г�ʼ����RabbitMQ Server�����ӡ������ص�queue����ʼ�ڻص�queue�ϵȴ�������Ӧ���������ڻص�queue�Ͻ��յ���Ӧ��Ĵ�����on_response������Ӧ������correlation_id����������Ӧ�������˵��ú����������������queue���Ͱ���correlation_id�����Եĵ������󡢳�ʼ��һ��clientʵ������30Ϊ��������Զ�̹��̵��á�
python rpc_server.py
python rpc_client.py


rabbitmq_comands.sh
    ������rabbitmq��һЩ�����в���


rabbitmq_clusters.sh
    ��δ���cluster
    cluster��һЩ��������


rabbit_high_availability.sh


rabbitmq_web_stomp.sh
    ��web������


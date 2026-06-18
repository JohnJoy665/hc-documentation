# WebTutor / WebSoft HCM: база данных, объектная модель, XMD и каталоги

> Источник: присланный текст из документации WebTutor. Материал структурирован в Markdown без добавления внешних сведений.

## Оглавление

- Поддерживаемые типы баз данных
- Общие принципы хранения данных
- Пример XML-структуры объекта
- XMD-файлы для объектов
- Каталог `person_hierarchys`
- Справочник каталогов
- Данные каталога `websoft.hcm.collaborator`

## Поддерживаемые типы баз данных

Система WebTutor поддерживает работу со следующими типами баз данных:

- Microsoft SQL Server (начиная с 2008, оптимально 2012)
- Oracle (11+)
- PostgreSQL
- XML (внутреннее хранилище)
- Microsoft Azure (при облачном размещении)

## Общие принципы хранения данных в системе

На логическом уровне база данных WebTutor строится на основе объектной модели.

В базе данных системы хранятся объекты различных типов. Примеры типов объектов:

- Сотрудник
- Электронный курс
- Компетенция
- ...

Любой объект представляет собой иерархическую структуру данных. У любого объекта есть XML-представление.

Атрибуты в документе могут быть:

- Скалярными — текст, число, дата, булевское значение
- Массивами — содержащими скалярные значения или другие массивы
- Ссылками на другие объекты в базе

Структура (набор атрибутов) объектов определенного типа описана в XMD-документе. Эти документы включены в дистрибутив системы WebTutor и обновляются при установке дистрибутива системы.

Для каждого типа объекта существует как минимум один каталог (таблица), по которой возможен поиск объектов с помощью XQuery-запросов. Структура (набор атрибутов) каталогов также описана в XMD-документе.

При этом структура (набор атрибутов) каталога может отличаться от структуры объекта.

Физическая структура базы может различаться для различных типов СУБД, поддерживаемых WebTutor.

После установки дистрибутива, при первом запуске сервера WebTutor, физическая структура данных системы актуализируется на основе актуальных XMD-файлов.

## Пример XML-структуры объекта

Дата создания: 17.04.2019.

```xml
<objects><lector>
    <id>0x52936EC7753C7D09</id>
    <type>collaborator</type>
    <desc>&lt;p&gt;&lt;span style="font-size: small;"&gt;Генеральный директор компании Websoft. Более 15 лет опыта автоматизации HR-процессов и организации электронного обучения.&lt;/span&gt;&lt;/p&gt;</desc>
    <person_id>0x51E0E60C1881209B</person_id>
    <person_fullname>Корольков Алексей</person_fullname>
    <person_position_name>Генеральный директор</person_position_name>
    <person_subdivision_name>WebSoft</person_subdivision_name>
    <allow_publication>0</allow_publication>
    <is_dismiss>0</is_dismiss>
    <doc_info>
        <creation>
            <user_login>user1</user_login>
            <user_id>0x5555555555555501</user_id>
            <date>2013-11-25T20:11:08+00:00</date>
        </creation>
        <modification>
            <user_login>akor</user_login>
            <user_id>0x51E0E60C1881209B</user_id>
            <date>2015-04-07T15:30:32+00:00</date>
        </modification>
    </doc_info>
    <access>
        <access_level>0</access_level>
    </access>
</lector>
</objects>
```

## XMD-файлы для объектов

Дата создания: 17.04.2019.

XMD-файлы, описывающие структуру типов объектов, хранятся в папке сервера системы по адресу:

```text
\wtv\wtv_<тип объекта>.xmd
```

Пример XMD-файла:

```xml
<?xml version="1.0" encoding="utf-8"?>
<SPXML-FORM>

<USE FORM="//wtv/ms_general.xmd"/>

<lector>
    <id TYPE="integer" TITLE="ID"/>
    <code TYPE="string" TITLE="const=c_code"/>
    <type TYPE="string" NOT-NULL="1" DEFAULT="invitee" FOREIGN-ARRAY="common.lector_types" TITLE="const=c_type"/><!-- invitee, collaborator -->
    <desc TYPE="string" TITLE="const=c_desc"/>

    <person_id TYPE="integer" FOREIGN-ARRAY="collaborators" TITLE="const=c_coll"/>
    <person_fullname TYPE="string" TITLE="const=lhbyv18qkm"/>
    <person_position_name TYPE="string" TITLE="const=c_position"/>
    <person_subdivision_name TYPE="string" TITLE="const=c_subd"/>

    <INHERIT TYPE="person_base"/>
    <INHERIT TYPE="passport_data_base"/>

    <allow_publication TYPE="bool" NOT-NULL="1" DEFAULT="false" TITLE="const=vleb_can_public"/>
    <is_dismiss TYPE="bool" NOT-NULL="1" DEFAULT="false" TITLE="const=3i9j58qey7"/>

    <doc_info TYPE="doc_info_base" TITLE="const=dseglc23jx"/>

    <INHERIT TYPE="custom_elems_base"/>

    <access TITLE="const=bmlkskx7us">
        <INHERIT TYPE="access_doc_base"/>
    </access>

    <INHERIT TYPE="admin_access_base"/>

    <INHERIT TYPE="path_subs_base"/>

    <view TEMP="1">
        <filter TEMP="1">
            <INHERIT TYPE="au_ft_filter" TEMP="1"/>
        </filter>
        <INHERIT TYPE="desc_base"/>
    </view>

    <lector_fullname PROPERTY="1" EXPR="
            return ( type == 'collaborator' ? person_fullname : fullname );
    "/>
</lector>

<OnBeforeSave PROPERTY="1" EXPR="
        teCollab = null;
        if( TopElem.person_id.HasValue )
            try
            {
                teCollab = OpenDoc( UrlFromDocID( TopElem.person_id ) ).TopElem;
            }
            catch( ex ){}
        tools.common_filling( 'collaborator', TopElem, TopElem.person_id, teCollab );

        TopElem.path_subs.Clear();
        if ( TopElem.type ==  'collaborator')
        {
            //Удаление полей с инфой о внешнем преподавателе
            TopElem.lastname.Clear();
            TopElem.firstname.Clear();
            TopElem.middlename.Clear();
            TopElem.sex.Clear();
            TopElem.birth_date.Clear();
            
            if( teCollab != null )
                TopElem.email = teCollab.email;

            //Заполнение массива штатного расписания
            if ( global_settings.settings.fill_path_subs &amp;&amp; TopElem.person_id.HasValue )
                tools.path_subs_filling( TopElem.path_subs, TopElem.person_id, null );
        }
        else
        {
            //Удаление полей с инфой о внутреннем преподавателе - сотруднике компании
            TopElem.person_id.Clear();
            TopElem.person_fullname.Clear();
            TopElem.person_position_name.Clear();
            TopElem.person_subdivision_name.Clear();
        }
"/>

<DocDesc PROPERTY="1" EXPR="
        ms_tools.get_const('c_lector') + ': ' + ( lector.person_fullname == '' ? lector.fullname : lector.person_fullname )
"/>

</SPXML-FORM>
```

## Каталог иерархической информации о сотрудниках `person_hierarchys`

Дата создания: 11.01.2022. Обновлено: 17.01.2022.

Каталог `person_hierarchys` предназначен для быстрого поиска сотрудников, имеющих отношение к указанному подразделению. Поиск может производиться иерархически (то есть не только в указанном, но и во всех дочерних подразделениях) или неиерархически (только в указанном подразделении).

Каталог `person_hierarchys` заполняется автоматически и регулярно обновляется.

Более подробно особенности работы с каталогом `person_hierarchys` описаны в исходной документации по ссылке «здесь» (ссылка в присланном тексте не раскрыта).

### Поля каталога `person_hierarchys`

| Поле | Описание |
| --- | --- |
| id | ID записи в каталоге `person_hierarchys` (целое число). |
| collaborator_id | ID сотрудника (целое число). |
| position_id | ID должности (целое число). |
| parent_sub_id | ID родительского подразделения (целое число). |
| subdivision_id | XML-структура, содержащая ID текущего подразделения и всех подразделений вверх по иерархии (все родительские подразделения последовательно, вплоть до самого верхнего) (объект `XmlElem`). |
| ready | Флаг актуальности записи (булево). Данное поле получает значение `false`, когда указанный сотрудник перемещен в оргструктуре (в т.ч. и вместе с подразделением), иначе говоря, когда изменение оргструктуры затронуло указанного сотрудника, но обработчик события изменения еще не успел пересчитать данную запись. На базах с большим количеством сотрудников такой перерасчет может занимать продолжительное время. |

### Примеры программного кода для работы с каталогом

```javascript
XQuery( "for $hier in person_hierarchys return $hier" );
XQuery( "for $hier in person_hierarchys where MatchSome($hier/subdivision_id,(" + ArrayMerge(aCatalogFMs, "This.parent_id.Value", ",") + ")) and $hier/collaborator_id != " + iPersonID + " return $hier/Fields('collaborator_id')" ); // иерархический отбор
```

## Справочник каталогов

Версия: 1.26.05.21 (2026-05-21).

| Каталог | Описание |
| --- | --- |
| action_report | Событие базы |
| active_notification | Неотправленное уведомление |
| active_web_template | Текущая сессия портала |
| admin_configuration | Конфигурация |
| ai_model | AI модель |
| application | Приложение |
| application_instance | Процесс приложения |
| banner | Баннер |
| block_message | Блоки сообщений |
| calendar_event | Событие календаря |
| calendar_event_type | typesobytiekalenda |
| call | Звонок |
| career_reserve | Развитие карьеры |
| career_reserve_type | Тип кадрового резерва |
| channel_provider | Провайдер канала |
| chat | Чат |
| chatbot | Чат-бот |
| chatbot_chat | Чат с чат-ботом |
| chatbot_stage | Этап чат-бота |
| chatbot_type | Тип чат-бота |
| classification_model | Модель классификации |
| code_library | Библиотека программного кода |
| coding | Нумератор |
| conversation | Разговоры |
| conversation_object_type | Типы объектов разговора |
| conversation_participant | Участник разговора |
| conversation_type | Типы разговоров |
| credential | Удостоверение |
| custom_chatbot_template | Настраиваемый шаблон для чат-ботов |
| custom_report | Настраиваемый отчет |
| custom_web_template | Настраиваемый Web-шаблон |
| custom_web_template_group | Группа Web-шаблонов |
| device_reg | Подключенное устройство |
| exchange_server | Сервер обмена данными |
| feature | funciya |
| file_source | Источники файлов |
| group | Группа |
| host | Узел |
| i18n_data | Локализационные данные |
| learning_part | Раздел курс |
| library_system | Система библиотек |
| license | Лицензия библиотеки |
| license_catalog | license_catalog |
| license_module | license_module |
| namespace | namespace |
| notification | Тип уведомления |
| notification_system | Система уведомлений |
| notification_template | Шаблон уведомления |
| object_data | Данные объектов |
| object_data_type | Тип данных объектов |
| object_param | Параметры объекта |
| object_version | Версия объекта |
| operation | Операция |
| override_web_template | Элемент шаблона |
| owner_object | Владелец объекта |
| person_state | sostoyaniesotrudnikov |
| personal_assistant | Персональный помощник |
| poll | Опрос |
| poll_procedure | Процедура опроса |
| poll_result | Результат опроса |
| print_form | Печатная форма |
| process_handler | Активный процесс |
| proctoring_system | Система прокторинга |
| product | product |
| reaction | Реакция на сообщение |
| recruitment_system | Система подбора персонала |
| remote_action | Удаленное действие |
| remote_application | Внешнее приложение API |
| remote_collection | Выборка |
| remote_security_profile | Профиль безопасности |
| repositorium | Репозиторий |
| repository_file | Файл репозитория |
| request | Заявка |
| request_type | Тип заявки |
| resource | Ресурс базы |
| resource_type | Вид файла |
| role | Категория |
| script_queue_elem | Очередь скриптов |
| server_agent | Агент сервера |
| site | Сайт |
| statistic_rec | Показатель |
| system_event | Системные события |
| system_event_handler | Обработчик системных событий |
| testing_system | Система тестирования |
| time_entry | Учет времени по работам |
| typical_development_program | Типовая программа развития |
| uni_client_config | Конфигурация универсального клиента |
| user_data | Пользовательские данные |
| user_web_state | User web state |
| vacancy | Вакансия |
| vacancy_reason | Причина вакансии |
| vacancy_response | Отклик на вакансию |
| vacancy_source | Источник вакансии |
| vacancy_subscription | Подписка на вакансии |
| vacancy_type | Тип вакансий |
| view_configuration | Представление |
| web_configuration | Конфигурации страниц |
| web_design | Дизайн портала |
| web_mode | Тип страницы |
| web_person_state | Статус пользователя на портале |
| web_rule | Правило |
| webinar_system | Система вебинаров |
| websoft.hcm.absence_reserve | Резерв на отсутствие |
| websoft.hcm.account | Билинговый счет |
| websoft.hcm.acquaint | Ознакомление |
| websoft.hcm.acquaint_assign | Назначенное ознакомление |
| websoft.hcm.active_learning | Незаконченный электронный курс |
| websoft.hcm.active_test_learning | Незаконченный тест |
| websoft.hcm.activity | Учебная активность |
| websoft.hcm.activity_state | Статус учебной активности |
| websoft.hcm.appointment_type | Тип назначения |
| websoft.hcm.assessment | Тест |
| websoft.hcm.availability_plan | План доступности |
| websoft.hcm.basket | Корзина |
| websoft.hcm.blog | Блог |
| websoft.hcm.blog_entry | Сообщение блога |
| websoft.hcm.blog_entry_comment | Коментарий сообщения блога |
| websoft.hcm.boss_type | Тип руководителя |
| websoft.hcm.budget | Бюджет |
| websoft.hcm.budget_period | Бюджетный период |
| websoft.hcm.budget_type | Тип бюджета |
| websoft.hcm.candidate_source | Источник кандидатов |
| websoft.hcm.candidate_status_type | Статус кандидата |
| websoft.hcm.career_plan | Карьерный план |
| websoft.hcm.career_reserve | Развитие карьеры |
| websoft.hcm.career_reserve_type | Тип кадрового резерва |
| websoft.hcm.certificate | Сертификат |
| websoft.hcm.certificate_type | Тип сертификата |
| websoft.hcm.change | Изменение |
| websoft.hcm.check_schedule | Проверка графика |
| websoft.hcm.cl_const | Константа |
| websoft.hcm.cl_course | Курс CourseLab |
| websoft.hcm.cl_localization | Локализация |
| websoft.hcm.cl_module | Модуль CourseLab |
| websoft.hcm.cl_object | Объект CourseLab |
| websoft.hcm.cl_shape | Автофигура CourseLab |
| websoft.hcm.cl_slide | Слайд CourseLab |
| websoft.hcm.cl_video_course | Видео-курс |
| websoft.hcm.client | client |
| websoft.hcm.closing_document | Закрывающий документ |
| websoft.hcm.closing_document_status | Статус закрывающего документа |
| websoft.hcm.collaborator | Сотрудник |
| websoft.hcm.collaborator_schedule | График сотрудника |
| websoft.hcm.committee_member | Участник кадрового комитета |
| websoft.hcm.compound_program | Модульная программа |
| websoft.hcm.contest | Конкурс |
| websoft.hcm.contract | Договор |
| websoft.hcm.contract_state | Статус договора |
| websoft.hcm.contract_type | Тип договора |
| websoft.hcm.cost_center | Центр затрат |
| websoft.hcm.course | Электронный курс |
| websoft.hcm.course_module | Учебный модуль |
| websoft.hcm.covenant | Ученический договор |
| websoft.hcm.development_potential | Потенциал развития |
| websoft.hcm.dimension | Размерность |
| websoft.hcm.document | Раздел портала |
| websoft.hcm.edm.attorney | Доверенность |
| websoft.hcm.edm.personnel_document | Кадровый документ |
| websoft.hcm.edm.personnel_document_type | Тип кадрового документа |
| websoft.hcm.education_form | Форма обучения |
| websoft.hcm.education_method | Учебная программа |
| websoft.hcm.education_mode | Способ получения образования |
| websoft.hcm.education_org | Обучающая организация |
| websoft.hcm.education_plan | План обучения |
| websoft.hcm.education_program | Набор программ |
| websoft.hcm.education_type | Тип образования |
| websoft.hcm.efficiency_estimation | Оценка эффективности |
| websoft.hcm.efficiency_grade | Оценка эффективности |
| websoft.hcm.estaff_event | Обработка события E-Staff |
| websoft.hcm.estaff_event_type | Тип события E-Staff |
| websoft.hcm.event | Мероприятие |
| websoft.hcm.event_assessment_plan | Оценка по мероприятию |
| websoft.hcm.event_result | Результат мероприятия |
| websoft.hcm.event_result_type | Тип результата мероприятия |
| websoft.hcm.event_type | Тип мероприятия |
| websoft.hcm.exclusion_reason | Основание для исключения |
| websoft.hcm.expense | Счет |
| websoft.hcm.expense_item | Статья затрат |
| websoft.hcm.expert | Эксперт |
| websoft.hcm.expert_question | Вопрос эксперту |
| websoft.hcm.fact_payment | Факт оплаты |
| websoft.hcm.forum | Форум |
| websoft.hcm.forum_entry | Статья форума |
| websoft.hcm.forum_theme_read_by_collaborator | Прочтение темы сотрудником |
| websoft.hcm.good | Товар |
| websoft.hcm.good_instance | Экземпляр товара |
| websoft.hcm.good_type | Тип товаров |
| websoft.hcm.grade | Грейд |
| websoft.hcm.group | Группа |
| websoft.hcm.interval_schedule | Интервалы графика |
| websoft.hcm.item | Вопрос |
| websoft.hcm.key_position | Ключевая должность |
| websoft.hcm.key_position_threat | Угроза ключевой должности |
| websoft.hcm.knowledge_acquaint | Подтверждение знаний |
| websoft.hcm.knowledge_classifier | Классификатор знаний |
| websoft.hcm.knowledge_part | Значение карты знаний |
| websoft.hcm.knowledge_part_level | Категория значения карты знаний |
| websoft.hcm.knowledge_part_type | Тип значение карты знаний |
| websoft.hcm.knowledge_profile | Профиль знаний |
| websoft.hcm.learning | Законченный электронный курс |
| websoft.hcm.learning_storage | Хранилище учебных записей |
| websoft.hcm.learning_task | Задание |
| websoft.hcm.learning_task_result | Выполненное задание |
| websoft.hcm.lector | Преподаватель |
| websoft.hcm.level | Уровень |
| websoft.hcm.library_material | Материал библиотеки |
| websoft.hcm.library_material_comment | Отзыв о материале |
| websoft.hcm.library_material_format | Формат материала |
| websoft.hcm.library_material_item | Экземпляр материала библиотеки |
| websoft.hcm.library_material_source_type | Тип входных данных |
| websoft.hcm.library_material_type | Вид материала |
| websoft.hcm.library_material_viewing | Просмотр материала библиотеки |
| websoft.hcm.library_player | Проигрыватель |
| websoft.hcm.library_section | Раздел библиотеки |
| websoft.hcm.library_system | Система библиотек |
| websoft.hcm.like | Like |
| websoft.hcm.lms.person_state | sostiniesotrudnikov |
| websoft.hcm.material_liability | Материальная ответственность |
| websoft.hcm.messenger_service | Курьерская служба |
| websoft.hcm.object_resource | Оборудование |
| websoft.hcm.order | Заказ |
| websoft.hcm.order_type | Установки для приказов по умолчанию |
| websoft.hcm.org | Организация |
| websoft.hcm.participant | Участник конкурса |
| websoft.hcm.pay_phase | Этап оплаты |
| websoft.hcm.pay_stage | Этап договора |
| websoft.hcm.person_object_link | Контент сотрудника |
| websoft.hcm.person_object_profile | Профиль редактирования контента |
| websoft.hcm.personal_data_processing_consent | Согласие на обработку персональных данных |
| websoft.hcm.personnel_committee | Кадровый комитет |
| websoft.hcm.personnel_event | personnelevent |
| websoft.hcm.personnel_event_type | personneleventtype |
| websoft.hcm.personnel_reserve | Кадровый резерв |
| websoft.hcm.place | Расположение |
| websoft.hcm.poll | Опрос |
| websoft.hcm.poll_procedure | Процедура опроса |
| websoft.hcm.poll_result | Результат опроса |
| websoft.hcm.position | Должность |
| websoft.hcm.position_common | Типовая должность |
| websoft.hcm.position_common_instruction | Должностная инструкция |
| websoft.hcm.position_family | Семейство должностей |
| websoft.hcm.potential_grade | Оценка потенциала |
| websoft.hcm.presence_state | Тип присутствия/отсутствия |
| websoft.hcm.price | Прайс |
| websoft.hcm.print_form | Печатная форма |
| websoft.hcm.profession | Профессия |
| websoft.hcm.profession_category | Разряд профессии |
| websoft.hcm.professional_area | Профессиональная область |
| websoft.hcm.professional_area_type | Тип профессиональной области |
| websoft.hcm.profile | Профиль |
| websoft.hcm.project.project | Проект |
| websoft.hcm.project.project_participant | Участник проекта |
| websoft.hcm.project.project_participant_role | Роль участника проекта |
| websoft.hcm.project.project_type | Тип проекта |
| websoft.hcm.provider | Продукт/услуга |
| websoft.hcm.provider_course_type | providerkursov |
| websoft.hcm.qualification | Квалификация |
| websoft.hcm.qualification_assignment | Присвоение квалификации |
| websoft.hcm.rating | Рейтинг |
| websoft.hcm.readiness_level | Уровень готовности |
| websoft.hcm.recommendation | Рекомендация на вакансию |
| websoft.hcm.recruitment_event | Событие |
| websoft.hcm.recruitment_method | Способ подбора |
| websoft.hcm.recruitment_plan | План подбора |
| websoft.hcm.region | Регион |
| websoft.hcm.reprimand | Дисциплинарное взыскание |
| websoft.hcm.reprimand_type | Тип взыскания |
| websoft.hcm.response | Отзыв |
| websoft.hcm.response_type | Тип отзыва |
| websoft.hcm.restricting_collaborator_schedule | Ограничение графика сотрудника |
| websoft.hcm.restricting_type | Тип ограничения |
| websoft.hcm.resume | Резюме |
| websoft.hcm.risk_level | Фактор риска |
| websoft.hcm.risk_perspective | Уровень риска |
| websoft.hcm.sale_contract | Договор |
| websoft.hcm.sale_contract_parameter | parameterdogovora_1 |
| websoft.hcm.sale_contract_type | Тип договора |
| websoft.hcm.sale_contract_type_parameter | Тип договора |
| websoft.hcm.schedule_type | Тип графика |
| websoft.hcm.section_instruction | Раздел инструкции |
| websoft.hcm.selected_resume | Отобранное резюме |
| websoft.hcm.skill_type | Тип навыка |
| websoft.hcm.social_network | socialnetwork |
| websoft.hcm.staff_position | Позиция штатного расписания |
| websoft.hcm.staff_position_finish | Увольнение |
| websoft.hcm.staff_position_move | Кадровое перемещение |
| websoft.hcm.staff_position_start | Прием на работу |
| websoft.hcm.statement | Сообщение учебной активности |
| websoft.hcm.statement_actor | Исполнитель учебной активности |
| websoft.hcm.statement_additional | Дополнительный параметр активности |
| websoft.hcm.statement_attachment | Вложение учебной активности |
| websoft.hcm.statement_component | Компонент активности |
| websoft.hcm.statement_object | Объект учебной активности |
| websoft.hcm.statement_pattern | Паттерн правильного ответа |
| websoft.hcm.subdivision | Подразделение |
| websoft.hcm.subdivision_group | Группа подразделений |
| websoft.hcm.subscription | Подписка |
| websoft.hcm.substitution | Замещение |
| websoft.hcm.substitution_type | Тип замещения |
| websoft.hcm.successor | Преемник |
| websoft.hcm.tag | Тег |
| websoft.hcm.tagged_object | tagged_object |
| websoft.hcm.talent_pool_nomination | Способ выдвижения в резерв |
| websoft.hcm.test_learning | Законченный тест |
| websoft.hcm.testing_system | Система тестирования |
| websoft.hcm.training_plan | План мероприятий |
| websoft.hcm.traning_order | Приказ на обучение |
| websoft.hcm.transaction | Транзакция |
| websoft.hcm.tutor | Наставник |
| websoft.hcm.typical_development_program | Типовая программа развития |
| websoft.hcm.vacancy | Вакансия |
| websoft.hcm.vacancy_reason | Причина вакансии |
| websoft.hcm.vacancy_response | Отклик на вакансию |
| websoft.hcm.vacancy_source | Источник вакансии |
| websoft.hcm.vacancy_subscription | Подписка на вакансии |
| websoft.hcm.vacancy_type | Тип вакансий |
| websoft.hcm.verb | Действие |
| websoft.hcm.wage_system | Система оплаты труда |
| websoft.hcm.webinar_system | Система вебинаров |
| websoft.hcm.wiki_article | Статья базы знаний |
| websoft.hcm.wiki_article_communication | Связь статей базы знаний |
| websoft.hcm.wiki_article_communication_type | Тип связей статей базы знаний |
| websoft.hcm.wiki_article_type | Тип статей базы знаний |
| websoft.hcm.wiki_base | База знаний |
| websoft.hcm.work_experience | Опыт работы |
| websoft.hcm.work_mode | Режим работы |
| websoft.hcm.work_schedule | График работы |
| websoft.platform.bpm.discharge | Выгрузки из базы |
| websoft.platform.bpm.qa_test | Тест программного кода |
| websoft.platform.bpm.qa_test_paramset | Набор параметров тестов программного кода |
| websoft.platform.bpm.qa_test_result | Результат выполнения теста программного кода |
| websoft.platform.bpm.qa_test_set | Набор тестов программного кода |
| websoft.platform.devtools.devtool_catalog | 'Каталоги' |
| websoft.platform.devtools.devtool_library_method | 'Функции библиотек' |
| websoft.platform.security.auth_type | Тип авторизации |
| websoft.platform.security.object_claim | Разрешение для объекта |
| websoft.platform.security.user | User |
| websoft.platform.security.user_type | User type |
| workflow | Тип процессов |
| workspace_config | Конфигурация WorkSpace |

## Данные каталога `websoft.hcm.collaborator` (`Сотрудник`)

| Поле | Тип поля | Описание | Ссылка на другой каталог |
| --- | --- | --- | --- |
| id | integer | ID |  |
| code | string | Код |  |
| fullname | string | ФИО |  |
| login | string | Логин |  |
| short_login | string | Логин без домена |  |
| lowercase_login | string | Логин в нижнем регистре |  |
| email | string | Email |  |
| phone | string | Телефон |  |
| mobile_phone | string | Мобильный телефон |  |
| birth_date | date | Дата рождения |  |
| birth_day | integer | День месяца рождения |  |
| birth_month | integer | Месяц рождения |  |
| sex | string | Пол |  |
| pict_url | string | URL к файлу фотографии |  |
| position_id | integer | Должность | websoft.hcm.positions |
| position_name | string | Название должности |  |
| position_parent_id | integer | Подразделение | websoft.hcm.subdivisions |
| position_parent_name | string | Название подразделения |  |
| org_id | integer | Организация | websoft.hcm.orgs |
| org_name | string | Название организации |  |
| place_id | integer | Расположение | websoft.hcm.places |
| region_id | integer | Регион | websoft.hcm.regions |
| web_banned | bool | Запрещен доступ на портал |  |
| is_arm_admin | bool | Является пользователем интерфейса администратора |  |
| is_content_admin | bool | Является редактором контента |  |
| is_application_admin | bool | Является пользователем приложений |  |
| role_id | string | Роль | access_roles |
| is_candidate | bool | Является кандидатом |  |
| candidate_status_type_id | integer | Статус кандидата | websoft.hcm.candidate_status_types |
| candidate_id | integer | Кандидат |  |
| is_outstaff | bool | Является временным |  |
| is_dismiss | bool | Является уволенным |  |
| position_date | date | Дата вступления в должность |  |
| hire_date | date | Дата приема |  |
| dismiss_date | date | Дата увольнения |  |
| in_request_black_list | bool | В "черном списке" на подачу заявок |  |
| allow_personal_chat_request | bool | Разрешить приглашение пользователя к общению |  |
| level_id | integer | Уровень компетентности | websoft.hcm.levels |
| grade_id | integer | Грейд | websoft.hcm.grades |
| knowledge_parts | string | Значения карты знаний |  |
| tags | string | ID тегов |  |
| experts | string | Эксперты |  |
| current_state | string | Текущее состояние |  |
| current_state_id | integer | ID Состояния сотрудника | websoft.hcm.person_states |
| next_state_date | date | Дата следующего изменения статуса сотрудника |  |
| avatar_filename | string | URL аватара (относительно [Server]/wt/web/avatars) |  |
| development_potential_id | integer | Потенциал развития | websoft.hcm.development_potentials |
| efficiency_estimation_id | integer | Оценка эффективности | websoft.hcm.efficiency_estimations |
| consent_kedo | bool | Согласие на КЭДО |  |
| consent_kedo_date | date | Дата согласия на КЭДО |  |
| provider_legal_id | string | Код юридического лица |  |
| snils | string | Снилс |  |
| cost_center_id | integer | Центр затрат | websoft.hcm.cost_centers |
| disp_birthdate | bool | Отображать дату рождения |  |
| disp_birthdate_year | bool | Отображать год рождения |  |
| modification_date | date | Дата модификации |  |
| app_instance_id | string | Код сервера |  |

## Примечания для последующей AI-friendly переработки

- В этом файле сохранены только сведения, присутствующие в присланном тексте.
- Иллюстрации, упомянутые в исходном тексте как «добавленная иллюстрация 1/2», отсутствуют в присланном материале.
- Ссылка «здесь» в описании `person_hierarchys` не раскрыта в присланном материале.
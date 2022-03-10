 4 app/dashboard/views/notification.py 
@@ -1,28 +1,32 @@
do  redirecionamento de importação do frasco  , url_for , flash , render_template , solicitação 
from  flask_login  import  login_required , current_user
do  aplicativo . importação de configuração  PAGE_LIMIT 
do  aplicativo . painel . importação base  dashboard_bp 
do  aplicativo . Sessão de importação de banco de dados  
do  aplicativo . Notificação de importação de modelos  
@painel_bp . _ route ( "/notification/<notification_id>" , métodos = [ "GET" , "POST" ])
@ login_required
def  notification_route ( notification_id ):
    notificação  =  Notificação . obter ( notification_id )
    se  não for  notificação :
        flash ( "Link incorreto. Redirecionar você para a página inicial" , "aviso" )
         redirecionamento de retorno ( url_for ( "dashboard.index" ))
    se  notificação . user_id  !=  current_user . identificação :
        piscar (
            "Você não tem acesso a esta página. Redirecionar você para a página inicial" ,
            "aviso" ,
        )
         redirecionamento de retorno ( url_for ( "dashboard.index" ))

    se  não for  notificação . leia :
        notificação . leia  =  Verdadeiro
        Sessão . cometer ()

    se  solicitar . método  ==  "POST" :
        notification_title  =  notificação . título  ou  notificação . mensagem [: 20 ]
        Notificação . excluir ( notification_id )
        Sessão . cometer ()
        flash ( f" { notification_title } foi excluído" , "sucesso" )
         redirecionamento de retorno ( url_for ( "dashboard.index" ))
    mais :
        return  render_template ( "dashboard/notification.html" , notification = notification )
@painel_bp . _ route ( "/notifications" , métodos = [ "GET" , "POST" ])
@ login_required
def  notifications_route ():
    página  =  0
    se  solicitar . argumentos . get ( "página" ):
        page  =  int ( request . args . get ( "page" ))
    notificações  = (
        Notificação . filter_by ( user_id = current_user . id )
        . order_by ( Notificação . read , Notification . created_at . desc ())
        . limit ( PAGE_LIMIT  +  1 )   # carrega mais um registro para saber se tem mais
        . deslocamento ( página  *  PAGE_LIMIT )
        . todos ()
    )
    return  render_template (
        "dashboard/notifications.html" ,
        notificações = notificações ,
        página = página ,
        last_page = len ( notificações ) <=  PAGE_LIMIT ,
    )

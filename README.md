import javax.mail.*;
import javax.mail.search.FlagTerm;
import javax.swing.*;
import java.util.Properties;


/**
 * Created with IntelliJ IDEA.
 * User: User1
 * Date: 4/30/12
 * Time: 1:33 PM
 * To change this template use File | Settings | File Templates.
 */
public class doit {

    public static void main(String[] args) {
        JFrame frame = new JFrame("ClearMail-DT");
        JPanel panel = new JPanel();
        JButton button = new JButton("Connect");
        JTextField server = new JTextField("", 20);
        JTextField user = new JTextField("",20);
        JTextField pass = new JTextField();
        panel.add(server);

        panel.add(user);
        panel.add(button);
        frame.add(panel);
        frame.setSize(400,400);
        frame.setVisible(true);
       // go(server,user,pass);
    }


    public static void go(String server, String user, String pass){

        Properties props = System.getProperties();
        props.setProperty("mail.store.protocol", "imaps");
        int unread = 0;

        try {
            Session session = Session.getDefaultInstance(props, null);
            Store store = session.getStore("imaps");
            store.connect(server, user, pass);
            System.out.println(store);

            Folder inbox = store.getFolder("Inbox");
            inbox.open(Folder.READ_WRITE);
            unread = inbox.getUnreadMessageCount();

            FlagTerm ft = new FlagTerm(new Flags(Flags.Flag.SEEN), false);
            Message messages[] = inbox.search(ft);


            for (Message message : messages) {

                message.setFlag(Flags.Flag.SEEN, true);
            }

             System.out.print(unread);
        } catch (NoSuchProviderException e) {
            e.printStackTrace();

        } catch (MessagingException e) {
            e.printStackTrace();

        }
        //return unread;

    }
    }


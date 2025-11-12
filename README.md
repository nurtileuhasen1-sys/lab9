# lab9
class SoundModule {
    void initialize() {
        System.out.println("Sound Module activated.");
    }
    void adjustLevel(int dbLevel) {
        System.out.println("Sound level set to " + dbLevel + " dB.");
    }
    void shutDown() {
        System.out.println("Sound Module deactivated.");
    }
}

class DisplayDevice {
    void powerOn() {
        System.out.println("Display Device powered on.");
    }
    void selectDisplayMode(String modeName) {
        System.out.println("Display mode set to " + modeName + ".");
    }
    void powerOff() {
        System.out.println("Display Device powered off.");
    }
}

class IlluminationControl {
    void switchOn() {
        System.out.println("Room lighting switched on.");
    }
    void setIntensity(int percent) {
        System.out.println("Light intensity adjusted to " + percent + "%.");
    }
    void switchOff() {
        System.out.println("Room lighting switched off.");
    }
}

class MediaCenterControl {
    private SoundModule audioUnit;
    private DisplayDevice visualUnit;
    private IlluminationControl lightUnit;

    public MediaCenterControl(SoundModule audio, DisplayDevice visual, IlluminationControl light) {
        this.audioUnit = audio;
        this.visualUnit = visual;
        this.lightUnit = light;
    }

    void beginPresentation(String userName) {
        System.out.println("\n*** " + userName + ": Starting Media Presentation ***");
        lightUnit.switchOn();
        lightUnit.setIntensity(10);
        audioUnit.initialize();
        audioUnit.adjustLevel(10);
        visualUnit.powerOn();
        visualUnit.selectDisplayMode("4K");
        System.out.println("*** Presentation is now running. ***");
    }

    void finishPresentation(String userName) {
        System.out.println("\n*** " + userName + ": Shutting Down Media Center ***");
        visualUnit.powerOff();
        audioUnit.shutDown();
        lightUnit.switchOff();
        System.out.println("*** Media Center is fully off. ***");
    }
}

public class FacadeMediaDemo {
    public static void main(String[] args) {
        SoundModule sound = new SoundModule();
        DisplayDevice screen = new DisplayDevice();
        IlluminationControl lights = new IlluminationControl();
        
        MediaCenterControl manager = new MediaCenterControl(sound, screen, lights);
        
        manager.beginPresentation("Hasen Nurtiley"); 
        System.out.println("-------------------------");
        manager.finishPresentation("Hasen Nurtiley");
    }
}
import java.util.ArrayList;
import java.util.List;

abstract class HierarchyNode {
    protected String label;
    public HierarchyNode(String label) {
        this.label = label;
    }
    public abstract void showStructure(int indent);
    
    public void attachNode(HierarchyNode node) {
        throw new UnsupportedOperationException();
    }
    public void detachNode(HierarchyNode node) {
        throw new UnsupportedOperationException();
    }
    public HierarchyNode retrieveChild(int index) {
        throw new UnsupportedOperationException();
    }
}

class DataFile extends HierarchyNode {
    public DataFile(String label) {
        super(label);
    }
    public void showStructure(int indent) {
        System.out.println("  ".repeat(indent) + "|- File: " + label);
    }
}

class DataContainer extends HierarchyNode {
    private List<HierarchyNode> childrenList = new ArrayList<>();
    public DataContainer(String label) {
        super(label);
    }
    
    public void attachNode(HierarchyNode node) {
        childrenList.add(node);
    }
    public void detachNode(HierarchyNode node) {
        childrenList.remove(node);
    }
    public HierarchyNode retrieveChild(int index) {
        return childrenList.get(index);
    }
    
    public void showStructure(int indent) {
        System.out.println("  ".repeat(indent) + "|- Folder: " + label);
        for (HierarchyNode child : childrenList) {
            child.showStructure(indent + 2);
        }
    }
}

public class CompositeStructureDemo {
    public static void main(String[] args) {
        DataContainer rootNode = new DataContainer("Project_Root");
        DataFile fileA = new DataFile("config.ini");
        DataFile fileB = new DataFile("README.md");
        
        DataContainer sourceFolder = new DataContainer("SourceCode");
        DataFile srcFile1 = new DataFile("Main.java");
        
        rootNode.attachNode(fileA);
        rootNode.attachNode(fileB);
        
        sourceFolder.attachNode(srcFile1);
        rootNode.attachNode(sourceFolder);
        
        rootNode.showStructure(0);
    }
}

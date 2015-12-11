# handler

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.PrintStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;

import org.xml.sax.Attributes;
import org.xml.sax.ErrorHandler;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

public class ShiftHandler extends DefaultHandler implements ErrorHandler{

	
	
	 PrintStream out;
	    // bit mask values for flags
	    
	
	List<Footer> myFooters;
	List<Header> myHeaders;
	List<App> myApps;

	private String tempVal;
	private String tempValu;
	private String tempValue;
	private String tempUom;

	// to maintain context
	private Footer tempFooter;
	private Header tempHeader;
	private App tempApp;
	

	public ShiftHandler() {
	
		myFooters = new ArrayList<Footer>();
		myHeaders = new ArrayList<Header>();
		myApps = new ArrayList<App>();
	}

	public void runExample() {
		parseDocument();
		printData();
	saveRecord(myApps, myHeaders, myFooters);
	}
	
	private void parseDocument() {

		// get a factory
		SAXParserFactory spf = SAXParserFactory.newInstance();
		
		try {

			// get a new instance of parser
			SAXParser sp = spf.newSAXParser();
			//File folder = new File("D:\\Mamta\\XML");
			File folder = new File("D:\\Mamta\\New folder\\mk");
			//File folder = new File("D:\\Mamta\\New folder\\xml");
			File[] listOfFiles = folder.listFiles();
			
			for (File list : listOfFiles) {
			//YourClassConstructor(list);
				sp.parse(list,this);
			System.out.println(list.getName());
			
			System.out.println("App ID : " +tempApp.getId());
			
			//System.out.println("App ID : " +tempApp.getId());
			}
			
			
			File infoFile=new File("D:\\SidhiVinayak\\3-12-2015\\TP\\src\\Error.log");

	    	FileOutputStream fout=new FileOutputStream(infoFile); // infoFile---> File whre u wanna write logs
	    	//final PrintStream out=new PrintStream(fout);
	    	out=new PrintStream(fout);
	    	out.println("hello");
	    	out.println(App.class.getName());
	    //	out.println(App.class.getTypeParameters().equals(myApps));
	    	//out.println(myApps.add(tempApp.getaPId()));
	    	
	    	for (File list : listOfFiles) {
	    		out.println(list.getName());
	    		out.println(tempApp.getId());
	    		for (App li : myApps) {
		    	   	out.println(li.getId());
		    	   	 	
	    	}
	    	}
	    	
	    //	out.println(printFields(myApps));
			
		/*	File f = new File("D:\\Mamta\\XML\\ACS.xml",true);
			PrintWriter pout = new PrintWriter(f);
			pout.println("yhehhefhef0");*/
		//	File f = new File("D:\\SidhiVinayak\\3-12-2015\\Sample\\src\\",true);
		//	PrintWriter pout = new PrintWriter(f);
			//pout.println("yhehhefhef0");
			// parse the file and also register this class for call backs
			// sp.parse("D:\\SidhiVinayak\\3-12-2015\\Sample\\src\\ACS.xml", this);
	//sp.parse("D:\\SidhiVinayak\\3-12-2015\\Sample\\src\\ACES.xml",this);
			//sp.parse("D:\\SidhiVinayak\\3-12-2015\\TP\\src\\acesALLTagAndAttributeRead.xml",this);
			//sp.parse("D:\\SidhiVinayak\\3-12-2015\\Sample\\src\\ACES_MAHLEOriginal_Filters.xml", this);
		} catch (SAXException se) {
			se.printStackTrace();
			out.println(se);
		} catch (ParserConfigurationException pce) {
			pce.printStackTrace();
			out.println(pce);
		} catch (IOException ie) {
			ie.printStackTrace();
			out.println(ie);
		}
	}

	/**
	 * Iterate through the list and print the contents
	 */
	private void printData() {

		System.out.println("No of RecordCount '" + myFooters.size() + "'.");
		Iterator<Footer> it = myFooters.iterator();
		while (it.hasNext()) {
			System.out.println(it.next().toString());
		}

		System.out.println("No of Headers '" + myHeaders.size() + "'.");
		Iterator<Header> itr = myHeaders.iterator();
		while (itr.hasNext()) {
			System.out.println(itr.next().toString());
		}

		System.out.println("No of Apps '" + myApps.size() + "'.");
		Iterator<App> itrtr = myApps.iterator();
		while (itrtr.hasNext()) {
			System.out.println(itrtr.next().toString());
		}

	}

	// Event Handlers
	public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
		// reset
		tempVal = "";
		

		if (qName.equalsIgnoreCase("Footer")) {
			// create a new instance of employee
			tempFooter = new Footer();
			// tempFooter.setType(attributes.getValue("type"));
		}
		if (qName.equalsIgnoreCase("Header")) {
			// create a new instance of employee
			tempHeader = new Header();
			// tempHeader.setType(attributes.getValue("type"));
		}
		if (qName.equalsIgnoreCase("App")) {
			// create a new instance of employee
			tempValu = "";
			tempValue = "";
			tempUom = "";
			tempApp = new App();
			tempApp.setId(Long.parseLong(attributes.getValue("id")));
			tempApp.setAction(attributes.getValue("action"));
			tempApp.setValidate(attributes.getValue("validate"));
			tempApp.setRef(attributes.getValue("ref"));
			//tempApp.setVersion(attributes.getValue("version"));
			//tempApp.setVersion(attributes.getValue("version"));
		
		}
		if (qName.equalsIgnoreCase("BaseVehicle")) {
			String id = attributes.getValue("id");
			tempApp.setBaseVehicleId(Long.parseLong(id));
		}
		if (qName.equalsIgnoreCase("EngineBase")) {
			tempApp.setEngineBaseId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("EngineDesignation")) {
			tempApp.setEngineDesignationId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("FuelDeliverySubType")) {
			tempApp.setFuelDeliverySubTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("Note")) {
			String id = attributes.getValue("id");
			if (id != null) {
				tempApp.setNoteId(Long.parseLong(id));
			}
			String lang = attributes.getValue("lang");
			if (lang != null) {
				tempApp.setNoteLang(lang);
			}
		}
		if (qName.equalsIgnoreCase("Part")) {
			tempApp.setPartBrandAAIAId(attributes.getValue("BrandAAIAID"));
		}
		if (qName.equalsIgnoreCase("Aspiration")) {
			tempApp.setAspirationId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("BedLength")) {
			tempApp.setBedLengthId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("BedType")) {
			tempApp.setBedTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("BodyNumDoors")) {
			tempApp.setBodyNumDoorsId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("BodyType")) {
			tempApp.setBodyTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("BrakeABS")) {
			tempApp.setBrakeABSId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("BrakeSystem")) {
			tempApp.setBrakeSystemId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("CylinderHeadType")) {
			tempApp.setCylinderHeadTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("DriveType")) {
			tempApp.setDriveTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("EngineMfr")) {
			tempApp.setEngineMfrId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("EngineVersion")) {
			tempApp.setEngineVersionId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("EngineVIN")) {
			tempApp.setEngineVINId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("FrontBrakeType")) {
			tempApp.setFrontBrakeTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("FrontSpringType")) {
			tempApp.setFrontSpringTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("FuelDeliverySubType")) {
			tempApp.setFuelDeliverySubTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("FuelDeliveryType")) {
			tempApp.setFuelDeliveryTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("FuelSystemControlType")) {
			tempApp.setFuelSystemControlTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("FuelSystemDesign")) {
			tempApp.setFuelSystemDesignId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("FuelType")) {
			tempApp.setFuelTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("IgnitionSystemType")) {
			tempApp.setIgnitionSystemTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("Make")) {
			tempApp.setMakeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("MfrBodyCode")) {
			tempApp.setMfrBodyCodeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("Model")) {
			tempApp.setModelId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("PowerOutput")) {
			tempApp.setPowerOutputId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("RearBrakeType")) {
			tempApp.setRearBrakeTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("RearSpringType")) {
			tempApp.setRearSpringTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("Region")) {
			tempApp.setRegionId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("SteeringType")) {
			tempApp.setSteeringTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("SteeringSystem")) {
			tempApp.setSteeringSystemId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("SubModel")) {
			tempApp.setSubModelId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("TransElecControlled")) {
			tempApp.setTransElecContolledId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("TransmissionMfr")) {
			tempApp.setTransmissionMfrId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("TransmissionMfrCode")) {
			tempApp.setTransmissionMfrCodeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("TransmissionBase")) {
			tempApp.setTransmissionBaseId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("TransmissionControlType")) {
			tempApp.setTransmissionControlTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("TransmissionNumSpeeds")) {
			tempApp.setTransmissionNumSpeedsId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("TransmissionType")) {
			tempApp.setTransmissionTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("ValvesPerEngine")) {
			tempApp.setValvesPerEngineId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("VehicleType")) {
			tempApp.setVehicleTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("WheelBase")) {
			tempApp.setWheelBaseId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("Years")) {
			tempApp.setYearsFrom(attributes.getValue("from"));
			tempApp.setYearsTo(attributes.getValue("to"));
		}
		if (qName.equalsIgnoreCase("PartType")) {
			tempApp.setPartTypeId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("Qual")) {
			tempApp.setQualId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("Param")) {
				String value = attributes.getValue("value");
				if (value != null) {
					tempApp.setqParamValue(value + "" + tempValue);
					tempValue = tempValue + "," + value;
				}
		
				String uom = attributes.getValue("uom");
				if (uom != null) {
					tempApp.setqParamUom(uom + "" + tempUom);
					tempUom = tempUom + "," + uom;
				}
				
				String altvalue = attributes.getValue("altvalue");
				if (altvalue != null) {
					tempApp.setqParamAltValue(altvalue);
				}
				String altuom = attributes.getValue("altuom");
				if (altuom != null) {
					tempApp.setqParamAltUom(altuom);
				}
		}
		if (qName.equalsIgnoreCase("Position")) {
			tempApp.setPositionId(Long.parseLong(attributes.getValue("id")));
		}
		if (qName.equalsIgnoreCase("Asset")) {
			String action = attributes.getValue("action");
			if (action != null) {
				tempApp.setAssetAction(action);
			}
			String id = attributes.getValue("id");
			if (id != null) {
				tempApp.setAssetId(Long.parseLong(id));
			}
			tempApp.setAssetRef(attributes.getValue("ref"));
			tempApp.setAssetValidate(attributes.getValue("validate"));
		}
	}

	public void characters(char[] ch, int start, int length)
			throws SAXException {
		tempVal = new String(ch, start, length);
		
	}

	public void endElement(String uri, String localName, String qName)
			throws SAXException {

		if (qName.equalsIgnoreCase("Footer")) {
			// add it to the list
			myFooters.add(tempFooter);
		} else if (qName.equalsIgnoreCase("RecordCount")) {
			tempFooter.setRecordCount(Integer.parseInt(tempVal));
		}
		if (qName.equalsIgnoreCase("Header")) {
			// add it to the list
			myHeaders.add(tempHeader);
		} else if (qName.equalsIgnoreCase("Company")) {
			tempHeader.setCompany(tempVal);
		} else if (qName.equalsIgnoreCase("SenderName")) {
			tempHeader.setSenderName(tempVal);
		} else if (qName.equalsIgnoreCase("SenderPhone")) {
			tempHeader.setSenderPhone(tempVal);
		} else if (qName.equalsIgnoreCase("TransferDate")) {
			tempHeader.setTransferDate(tempVal);
		} else if (qName.equalsIgnoreCase("BrandAAIAID")) {
			tempHeader.setBrandAAIAID(tempVal);
		} else if (qName.equalsIgnoreCase("DocumentTitle")) {
			tempHeader.setDocumentTitle(tempVal);
		} else if (qName.equalsIgnoreCase("EffectiveDate")) {
			tempHeader.setTransferDate(tempVal);
		} else if (qName.equalsIgnoreCase("SubmissionType")) {
			tempHeader.setSubmissionType(tempVal);
		} else if (qName.equalsIgnoreCase("MapperCompany")) {
			tempHeader.setMapperCompany(tempVal);
		} else if (qName.equalsIgnoreCase("MapperContact")) {
			tempHeader.setMapperContact(tempVal);
		} else if (qName.equalsIgnoreCase("MapperPhone")) {
			tempHeader.setMapperPhone(tempVal);
		} else if (qName.equalsIgnoreCase("MapperEmail")) {
			tempHeader.setMapperEmail(tempVal);
		} else if (qName.equalsIgnoreCase("VcdbVersionDate")) {
			tempHeader.setVcdbVersionDate(tempVal);
		} else if (qName.equalsIgnoreCase("QdbVersionDate")) {
			tempHeader.setQdbVersionDate(tempVal);
		} else if (qName.equalsIgnoreCase("PcdbVersionDate")) {
			tempHeader.setPcdbVersionDate(tempVal);
		} else if (qName.equalsIgnoreCase("ApprovedFor")) {
			tempHeader.setApprovedFor(tempVal);
		} else if (qName.equalsIgnoreCase("DocFormNumber")) {
			tempHeader.setDocFormNumber(tempVal);
		} else if (qName.equalsIgnoreCase("MapperPhoneExt")) {
			tempHeader.setMapperPhoneExt(tempVal);
		} else if (qName.equalsIgnoreCase("SenderPhoneExt")) {
			tempHeader.setSenderPhoneExt(tempVal);
		}
		if (qName.equalsIgnoreCase("App")) {
			// add it to the list
			myApps.add(tempApp);
		//	saveRecord(myApps);
		//	myApps = null;
			//myApps.clear();
		/*	if (myApps.size() == 500)
			{
				System.out.println("Size calc");
				
				
				System.out.println("store in list");
			//	myApps.clear();
				
				System.out.println("list clear");
			}*/
		} else if (qName.equalsIgnoreCase("id")) {
			tempApp.setId(Long.parseLong(tempVal));
			
		} else if (qName.equalsIgnoreCase("action")) {
			tempApp.setAction(tempVal);
		} else if (qName.equalsIgnoreCase("validate")) {
			tempApp.setValidate(tempVal);
		}else if (qName.equalsIgnoreCase("ref")) {
			tempApp.setRef(tempVal);
		}/* else if (qName.equalsIgnoreCase("version")) {
			tempApp.setVersion(tempVal);
		}*/
		else if (qName.equalsIgnoreCase("Note")) {
			tempApp.setNote(tempValu + " " + tempVal);
			tempValu = tempValu + tempVal;
		} else if (qName.equalsIgnoreCase("Note lang")) {
			tempApp.setNoteLang(tempVal);
		} else if (qName.equalsIgnoreCase("Qty")) {
			tempApp.setQty(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("PartType id")) {
			tempApp.setPartTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("Part BrandAAIAID")) {
			tempApp.setPartBrandAAIAId(tempVal);
		} else if (qName.equalsIgnoreCase("Aspiration id")) {
			tempApp.setAspirationId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("BedLength id")) {
			tempApp.setBedLengthId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("BedType id")) {
			tempApp.setBedTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("BodyNumDoors id")) {
			tempApp.setBodyNumDoorsId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("BodyType id")) {
			tempApp.setBodyTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("BrakeABS id")) {
			tempApp.setBrakeABSId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("BrakeSystem id")) {
			tempApp.setBrakeSystemId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("CylinderHeadType id")) {
			tempApp.setCylinderHeadTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("DriveType id")) {
			tempApp.setDriveTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("EngineMfr id")) {
			tempApp.setEngineMfrId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("EngineVersion id")) {
			tempApp.setEngineVersionId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("EngineVIN id")) {
			tempApp.setEngineVINId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("FrontBrakeType id")) {
			tempApp.setFrontBrakeTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("FrontSpringType id")) {
			tempApp.setFrontSpringTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("FuelDeliveryType id")) {
			tempApp.setFuelDeliveryTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("FuelSystemControlType id")) {
			tempApp.setFuelSystemControlTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("FuelSystemDesign id")) {
			tempApp.setFuelSystemDesignId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("FuelType id")) {
			tempApp.setFuelTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("IgnitionSystemType id")) {
			tempApp.setIgnitionSystemTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("Make id")) {
			tempApp.setMakeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("MfrBodyCode id")) {
			tempApp.setMfrBodyCodeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("Model id")) {
			tempApp.setModelId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("PowerOutput id")) {
			tempApp.setPowerOutputId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("RearBrakeType id")) {
			tempApp.setRearBrakeTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("RearSpringType id")) {
			tempApp.setRearSpringTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("Region id")) {
			tempApp.setRegionId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("SteeringType id")) {
			tempApp.setSteeringTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("SteeringSystem id")) {
			tempApp.setSteeringSystemId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("SubModel id")) {
			tempApp.setSubModelId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("TransElecControlled id")) {
			tempApp.setTransElecContolledId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("TransmissionMfr id")) {
			tempApp.setTransmissionMfrId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("TransmissionMfrCode id")) {
			tempApp.setTransmissionMfrCodeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("TransmissionBase id")) {
			tempApp.setTransmissionBaseId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("TransmissionControlType id")) {
			tempApp.setTransmissionControlTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("TransmissionNumSpeeds id")) {
			tempApp.setTransmissionNumSpeedsId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("TransmissionType id")) {
			tempApp.setTransmissionTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("ValvesPerEngine id")) {
			tempApp.setValvesPerEngineId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("VehicleType id")) {
			tempApp.setVehicleTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("WheelBase id")) {
			tempApp.setWheelBaseId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("from")) {
			tempApp.setYearsFrom(tempVal);
		} else if (qName.equalsIgnoreCase("to")) {
			tempApp.setYearsTo(tempVal);
		} else if (qName.equalsIgnoreCase("Position id")) {
			tempApp.setPositionId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("Part")) {
			tempApp.setPart(tempVal);
		} else if (qName.equalsIgnoreCase("BaseVehicle id")) {
			tempApp.setBaseVehicleId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("EngineBase id")) {
			tempApp.setEngineBaseId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("EngineDesignation id")) {
			tempApp.setEngineDesignationId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("FuelDeliverySubType id")) {
			tempApp.setFuelDeliverySubTypeId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("Note id")) {
			tempApp.setNoteId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("Note lang")) {
			tempApp.setNoteLang(tempVal);
		} else if (qName.equalsIgnoreCase("Qual id")) {
			tempApp.setQualId(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("value")) {
			tempApp.setqParamValue(tempVal);
		} else if (qName.equalsIgnoreCase("uom")) {
			tempApp.setqParamUom(tempVal);
		} else if (qName.equalsIgnoreCase("text")) {
			tempApp.setqPText(tempVal);
		}else if (qName.equalsIgnoreCase("altvalue")) {
			tempApp.setqParamAltValue(tempVal);
		} else if (qName.equalsIgnoreCase("altuom")) {
			tempApp.setqParamAltUom(tempVal);
		} else if (qName.equalsIgnoreCase("AssetItemOrder")) {
			tempApp.setAssetItemOrder(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("AssetItemRef")) {
			tempApp.setAssetItemRef(tempVal);
		} else if (qName.equalsIgnoreCase("AssetItemName")) {
			tempApp.setAssetItemName(tempVal);
		} else if (qName.equalsIgnoreCase("DisplayOrder")) {
			tempApp.setDisplayOrder(Long.parseLong(tempVal));
		} else if (qName.equalsIgnoreCase("MfrLabel")) {
			tempApp.setMfrLabel(tempVal);
		} else if (qName.equalsIgnoreCase("Company")) {
			tempHeader.setCompany(tempVal);
		}

		else if (qName.equalsIgnoreCase("action")) {
			tempApp.setAssetAction(tempVal);
		} else if (qName.equalsIgnoreCase("id")) {
			tempApp.setAssetId(Long.parseLong(tempVal));
		}
		
		
	}


	
	/* public void error(SAXParseException ex) throws SAXException {
		     System.out.println("ERROR: [at " + ex.getLineNumber() + "] " + ex + " "+ ex.getMessage());
  }
 public void fatalError(SAXParseException ex) throws SAXException {
    System.out.println("FATAL_ERROR: [at " + ex.getLineNumber() + "] " + ex	+ " "+ ex.getMessage());
  }
  public void warning(SAXParseException ex) throws SAXException {
    System.out.println("WARNING: [at " + ex.getLineNumber() + "] " + ex	+ " "+ ex.getMessage());
  }
	*/


	
	public void saveRecord(List<App> myApps, List<Header> myHeaders, List<Footer> myFooters) {
		Connection con = null;
		ResultSet rs = null;
		PreparedStatement prSt = null;
		ResultSet rs1 = null;

		String insertHdr = "INSERT INTO HDR "
				+ "(HId, approvedFor, brandAIAID, company, docFormNumber, documentTitle, effectiveDate,"
				+ " mapperCompany, mapperContact, mapperEmail, mapperPhone, mapperPhoneExt, pcdbVersionDate,"
				+ " qdbVersionDate, senderName, senderPhone, senderPhoneExt, submissionType, transferDate,"
				+ " vcdbVersionDate) VALUES (hdr_sequence.NEXTVAL, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?,"
				+ " ?, ?, ?, ?, ?, ?)";

		String insertFtr = "INSERT INTO FTR " 
				+ "(FId,R_Count) VALUES"
				+ "(ftr_sequence.NEXTVAL,?)";
		
	
		
		//=============================================================================
		String insertApp = "INSERT INTO App "
			+ "(APID, action, id, ref, aValidate, aspirationId , assetAction,"
			+ " assetId, assetRef, assetValidate, assetItemOrder, assetItemRef, assetItemName,"
			+ " baseVehicleId, bedLengthId, bedTypeId, bodyNumDoorsId, bodyTypeId, brakeABSId,"
			+ " brakeSystemId, cylinderHeadTypeId, displayOrder, driveTypeId,"
			+ " engineBaseId, engineDesignationId, engineMfrId, engineVersionId,"
			+ " engineVINId, frontBrakeTypeId, frontSpringTypeId, fuelDeliverySubTypeId,"
			+ " fuelDeliveryTypeId, fuelSystemControlTypeId, fuelSystemDesignId, fuelTypeId,"
			+ " ignitionSystemTypeId, makeId, mfrLabel, mfrBodyCodeId,"
			+ " modelId, note, noteId, noteLang, part,"
			+ " partBrandAAIAId, partTypeId, positionId, powerOutputId, qualId,"
			+ " qParamUom, qParamValue, qParamAltValue, qParamAltUom, qPText, qty, rearBrakeTypeId,"
			+ " rearSpringTypeId, regionId, steeringTypeId, steeringSystemId, subModelId,"
			+ " transElecContolledId, transmissionMfrId, transmissionMfrCodeId,"
			+ " transmissionBaseId, transmissionControlTypeId, transmissionNumSpeedsId,"
			+ " transmissionTypeId, valvesPerEngineId, vehicleTypeId, wheelBaseId,"
			+ " yearsFrom, yearsTo, headerId,footerId ) VALUES (app_sequence.NEXTVAL, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?,"
			+ " ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?,"
			+ " ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?,?,?,?,?,?,?)";
		//============================================================================

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			System.out.println("Loding Driver Class");
			// step2 create the connection object
			con = DriverManager.getConnection("jdbc:oracle:thin:@192.168.2.68:1521:orcl", "test", "test");
			System.out.println("Connection created");
			// step3 create the statement object

			/*
			 * String query1 =
			 * "CREATE SEQUENCE ftr_sequence INCREMENT BY 1 START WITH 1 NOMAXVALUE NOCYCLE NOCACHE"; 
			 * String query2 =
			 * "CREATE SEQUENCE hdr_sequence INCREMENT BY 1 START WITH 1 NOMAXVALUE NOCYCLE NOCACHE"; 
			 * String query3 =
			 * "CREATE SEQUENCE app_sequence INCREMENT BY 1 START WITH 1 NOMAXVALUE NOCYCLE NOCACHE"; 
			 * prSt = con.prepareStatement(query1); 
			 * prSt = con.prepareStatement(query2); 
			 * prSt = con.prepareStatement(query3); 
			 * prSt.executeQuery(); 
			 * System.out.println("Creating Sequence");
			 */

			/*
			 * String query1 =
			 * "CREATE TABLE FTR(FID INT NOT NULL, R_COUNT INT, PRIMARY KEY (fID))";
			 * prSt = con.prepareStatement(query1);
			 * prSt.executeQuery();
			 * System.out.println("Footer Table created");
			 */

			/*
			 * String query2 =  "CREATE TABLE HDR(HID INT NOT NULL, approvedFor varchar(10)," 
			 * + brandAIAID varchar(10), company varchar(30), docFormNumber varchar(10),"
			 * + documentTitle varchar(30), effectiveDate varchar(30),  mapperCompany varchar(50),"
			 * + mapperContact varchar(30), mapperEmail varchar(30), mapperPhone varchar(20),"
			 * + mapperPhoneExt varchar(10), pcdbVersionDate varchar(30), qdbVersionDate varchar(30),"
			 * + senderName varchar(30), senderPhone varchar(20), senderPhoneExt varchar(10),"
			 * + submissionType varchar(10), transferDate varchar(30), vcdbVersionDate varchar(30), PRIMARY KEY (HID))"; 
			 * prSt = con.prepareStatement(query2);
			 * prSt.executeQuery();
			 * System.out.println("Header Table created");
			 */
/*
			
			  String query3 = "CREATE TABLE App(APID number NOT NULL, action varchar(5), id number, ref varchar(30)," +
			  		" aValidate varchar(10) DEFAULT 'Yes', aspirationId number, assetAction varchar(10),  assetId number," +
			  		" assetRef varchar(10), assetValidate varchar(10) DEFAULT 'Yes', assetItemOrder number," +
			  		" assetItemRef varchar(10), assetItemName varchar(20), baseVehicleId number," +
			  		" bedLengthId number, bedTypeId number, bodyNumDoorsId number, bodyTypeId number, brakeABSId number," +
			  		" brakeSystemId number, cylinderHeadTypeId number, displayOrder number, driveTypeId number," +
			  		" engineBaseId number, engineDesignationId number, engineMfrId number, engineVersionId number," +
			  		" engineVINId number, frontBrakeTypeId number, frontSpringTypeId number, fuelDeliverySubTypeId number," +
			  		" fuelDeliveryTypeId number, fuelSystemControlTypeId number, fuelSystemDesignId number, fuelTypeId number," +
			  		" ignitionSystemTypeId number, makeId number, mfrLabel varchar(30), mfrBodyCodeId number," +
			  		" modelId number, note varchar(200), noteId number, noteLang varchar(10), part varchar(30)," +
			  		" partBrandAAIAId varchar(30), partTypeId number, positionId number, powerOutputId number, qualId number," +
			  		" qParamUom varchar(20), qParamValue varchar(50), qParamAltValue varchar(20), qParamAltUom varchar(50), qPText varchar(20), qty number, rearBrakeTypeId number," +
			  		" rearSpringTypeId number, regionId number, steeringTypeId number, steeringSystemId number, subModelId number," +
			  		" transElecContolledId number, transmissionMfrId number, transmissionMfrCodeId number," +
			  		" transmissionBaseId number, transmissionControlTypeId number, transmissionNumSpeedsId number," +
			  		" transmissionTypeId number, valvesPerEngineId number, vehicleTypeId number, wheelBaseId number," +
			  		" yearsFrom varchar(10), yearsTo varchar(10), headerId number, footerId number," +
			  		" PRIMARY KEY (APID))";
			  prSt = con.prepareStatement(query3);
			  prSt.executeQuery();
			 System.out.println("App Table created");
			
			*/
			prSt = con.prepareStatement(insertFtr); 
			{
			prSt.setLong(1,(tempFooter.getRecordCount()));
			prSt.addBatch();
			}
			prSt.executeBatch();
			System.out.println("Record added in Footer");
			
			prSt = con.prepareStatement(insertHdr); 
			{
			prSt.setString(1,(tempHeader.getApprovedFor()));
			prSt.setString(2,(tempHeader.getBrandAAIAID())); 
			prSt.setString(3,(tempHeader.getCompany())); 
			prSt.setString(4,(tempHeader.getDocFormNumber())); 
			prSt.setString(5,(tempHeader.getDocumentTitle())); 
			prSt.setString(6,(tempHeader.getEffectiveDate())); 
			prSt.setString(7,(tempHeader.getMapperCompany())); 
			prSt.setString(8,(tempHeader.getMapperContact())); 
			prSt.setString(9,(tempHeader.getMapperEmail())); 
			prSt.setString(10,(tempHeader.getMapperPhone())); 
			prSt.setString(11,(tempHeader.getMapperPhoneExt())); 
			prSt.setString(12,(tempHeader.getPcdbVersionDate())); 
			prSt.setString(13,(tempHeader.getQdbVersionDate())); 
			prSt.setString(14,(tempHeader.getSenderName())); 
			prSt.setString(15,(tempHeader.getSenderPhone())); 
			prSt.setString(16,(tempHeader.getSenderPhoneExt())); 
			prSt.setString(17,(tempHeader.getSubmissionType())); 
			prSt.setString(18,(tempHeader.getTransferDate())); 
			prSt.setString(19,(tempHeader.getVcdbVersionDate())); 
			prSt.addBatch();
			}
			prSt.executeBatch();
			System.out.println("Record added in Header");
			
			int s = 0;
			int head = 0;
			int s1 = 0;
			int foot = 0;
			rs = con.createStatement().executeQuery("select MAX(HId) from HDR");
			if (rs != null) {
				while (rs.next()) {
					head = rs.getInt(1);
				}
			}
			System.out.println("Header Id = " + head);
			s = head;
			rs1 = con.createStatement()
					.executeQuery("select MAX(FID) from FTR");
			if (rs1 != null) {
				while (rs1.next()) {
					foot = rs1.getInt(1);
				}
			}
			int x=0;
			s1 = foot;
			int insert =0;
			int duplicate = 0;
			Statement stmt = con.createStatement();
			// int s1 = stmt.executeUpdate();
			System.out.println("Footer Id = " + s1);
			Iterator<App> i = myApps.iterator();

			while (i.hasNext()) {
				App ap = (App)i.next();
				
				String storeSql = "SELECT action, id, ref, aValidate, aspirationId , assetAction,"
					+ " assetId, assetRef, assetValidate, assetItemOrder, assetItemRef, assetItemName,"
					+ " baseVehicleId, bedLengthId, bedTypeId, bodyNumDoorsId, bodyTypeId, brakeABSId,"
					+ " brakeSystemId, cylinderHeadTypeId, displayOrder, driveTypeId,"
					+ " engineBaseId, engineDesignationId, engineMfrId, engineVersionId,"
					+ " engineVINId, frontBrakeTypeId, frontSpringTypeId, fuelDeliverySubTypeId,"
					+ " fuelDeliveryTypeId, fuelSystemControlTypeId, fuelSystemDesignId, fuelTypeId,"
					+ " ignitionSystemTypeId, makeId, mfrLabel, mfrBodyCodeId,"
					+ " modelId, note, noteId, noteLang, part, partBrandAAIAId, partTypeId, positionId, powerOutputId, qualId,"
					+ " qParamUom, qParamValue, qParamAltValue, qParamAltUom, qPText, qty, rearBrakeTypeId,"
					+ " rearSpringTypeId, regionId, steeringTypeId, steeringSystemId, subModelId,"
					+ " transElecContolledId, transmissionMfrId, transmissionMfrCodeId,"
					+ " transmissionBaseId, transmissionControlTypeId, transmissionNumSpeedsId,"
					+ " transmissionTypeId, valvesPerEngineId, vehicleTypeId, wheelBaseId,"
					+ " yearsFrom, yearsTo From App where ";
				if(ap.getAction() != null)
					 storeSql +=" action = '"+ap.getAction()+"'";
				if(ap.getId() != 0)
					 storeSql +=" and id = "+ap.getId();
				if(ap.getRef() != null)
					 storeSql +=" and ref = '"+ap.getRef()+"'";
				if(ap.getValidate() != null)
					 storeSql +=" and aValidate = '"+ap.getValidate()+"'";
				if(ap.getAspirationId() != 0)
					 storeSql +=" and aspirationId = "+ap.getAspirationId();
				if(ap.getAssetAction() != null)
					 storeSql +=" and assetAction = '"+ap.getAssetAction()+"'";
				if(ap.getAssetId() != 0)
					 storeSql +=" and assetId = "+ap.getAssetId();
				if(ap.getAssetRef() != null)
					 storeSql +=" and assetRef = '"+ap.getAssetRef()+"'";
				if(ap.getAssetValidate() != null)
					 storeSql +=" and assetValidate = '"+ap.getAssetValidate()+"'";
				if(ap.getAssetItemOrder() != 0)
					 storeSql +=" and assetItemOrder = "+ap.getAssetItemOrder();
				if(ap.getAssetItemRef() != null)
					 storeSql +=" and assetItemRef = '"+ap.getAssetItemRef()+"'";
				if(ap.getAssetItemName() != null)
					 storeSql +=" and assetItemName = '"+ap.getAssetItemName()+"'";
				if(ap.getBaseVehicleId() != 0)
					 storeSql +=" and baseVehicleId = "+ap.getBaseVehicleId();
				if(ap.getBedLengthId() != 0)
					 storeSql +=" and bedLengthId = "+ap.getBedLengthId();
				if(ap.getBedTypeId() != 0)
					 storeSql +=" and bedTypeId = "+ap.getBedTypeId();
				if(ap.getBodyNumDoorsId() != 0)
					 storeSql +=" and bodyNumDoorsId = "+ap.getBodyNumDoorsId();
				if(ap.getBodyTypeId() != 0)
					 storeSql +=" and bodyTypeId = "+ap.getBodyTypeId();
				if(ap.getBrakeABSId() != 0)
					 storeSql +=" and brakeABSId = "+ap.getBrakeABSId();
				if(ap.getBrakeSystemId() != 0)
					 storeSql +=" and brakeSystemId = "+ap.getBrakeSystemId();
				if(ap.getCylinderHeadTypeId() != 0)
					 storeSql +=" and cylinderHeadTypeId = "+ap.getCylinderHeadTypeId();
				if(ap.getDisplayOrder() != 0)
					 storeSql +=" and displayOrder = "+ap.getDisplayOrder();
				if(ap.getDriveTypeId() != 0)
					 storeSql +=" and driveTypeId = "+ap.getDriveTypeId();
				if(ap.getEngineBaseId()  != 0)
					 storeSql +=" and engineBaseId = "+ap.getEngineBaseId();
				if(ap.getEngineDesignationId() != 0)
					 storeSql +=" and engineDesignationId = "+ap.getEngineDesignationId();
				if(ap.getEngineMfrId() != 0)
					 storeSql +=" and engineMfrId = "+ap.getEngineMfrId();
				if(ap.getEngineVersionId() != 0)
					 storeSql +=" and engineVersionId = "+ap.getEngineVersionId();
				if(ap.getEngineVINId() != 0)
					 storeSql +=" and engineVINId = "+ap.getEngineVINId();
				if(ap.getFrontBrakeTypeId() != 0)
					 storeSql +=" and frontBrakeTypeId = "+ap.getFrontBrakeTypeId();
				if(ap.getFrontSpringTypeId() != 0)
					 storeSql +=" and frontSpringTypeId = "+ap.getFrontSpringTypeId();
				if(ap.getFuelDeliverySubTypeId() != 0)
					 storeSql +=" and fuelDeliverySubTypeId = "+ap.getFuelDeliverySubTypeId();
				if(ap.getFuelDeliveryTypeId() != 0)
					 storeSql +=" and fuelDeliveryTypeId = "+ap.getFuelDeliveryTypeId();
				if(ap.getFuelSystemControlTypeId() != 0)
					 storeSql +=" and fuelSystemControlTypeId = "+ap.getFuelSystemControlTypeId();
				if(ap.getFuelSystemDesignId() != 0)
					 storeSql +=" and fuelSystemDesignId = "+ap.getFuelSystemDesignId();
				if(ap.getFuelTypeId() != 0)
					 storeSql +=" and fuelTypeId = "+ap.getFuelTypeId();
				if(ap.getIgnitionSystemTypeId() != 0)
					 storeSql +=" and ignitionSystemTypeId = "+ap.getIgnitionSystemTypeId();
				if(ap.getMakeId() != 0)
					 storeSql +=" and makeId = "+ap.getMakeId();
				if(ap.getMfrLabel() != null)
					 storeSql +=" and mfrLabel = '"+ap.getMfrLabel()+"'";
				if(ap.getMfrBodyCodeId() != 0)
					 storeSql +=" and mfrBodyCodeId = "+ap.getMfrBodyCodeId();
				if(ap.getModelId() != 0)
					 storeSql +=" and modelId = "+ap.getModelId();
				if(ap.getNote() != null)
					 storeSql +=" and note = '"+ap.getNote()+"'";
				if(ap.getNoteId() != 0)
					 storeSql +=" and noteId = "+ap.getNoteId();
				if(ap.getNoteLang() != null)
					 storeSql +=" and noteLang = '"+ap.getNoteLang()+"'";
				if(ap.getPart() != null)
					 storeSql +=" and part = '"+ap.getPart()+"'";
				if(ap.getPartBrandAAIAId() != null)
					 storeSql +=" and partBrandAAIAId = '"+ap.getPartBrandAAIAId()+"'";
				if(ap.getPartTypeId() != 0)
					 storeSql +=" and partTypeId = "+ap.getPartTypeId();
				if(ap.getPositionId() != 0)
					 storeSql +=" and positionId = "+ap.getPositionId();
				if(ap.getPowerOutputId() != 0)
					 storeSql +=" and powerOutputId = "+ap.getPowerOutputId();
				if(ap.getQualId() != 0)
					 storeSql +=" and qualId = "+ap.getQualId();
				if(ap.getqParamUom() != null)
					 storeSql +=" and qParamUom = '"+ap.getqParamUom()+"'";
				if(ap.getqParamValue() != null)
					 storeSql +=" and qParamValue = '"+ap.getqParamValue()+"'";
				if(ap.getqParamAltValue() != null)
					 storeSql +=" and qParamAltValue = '"+ap.getqParamAltValue()+"'";
				if(ap.getqParamAltUom() != null)
					 storeSql +=" and qParamAltUom = '"+ap.getqParamAltUom()+"'";
				if(ap.getqPText() != null)
					 storeSql +=" and qPText = '"+ap.getqPText()+"'";
				if(ap.getQty() != 0)
					 storeSql +=" and qty = "+ap.getQty();
				if(ap.getRearBrakeTypeId() != 0)
					 storeSql +=" and rearBrakeTypeId= "+ap.getRearBrakeTypeId();
				if(ap.getRearSpringTypeId() != 0)
					 storeSql +=" and rearSpringTypeId = "+ap.getRearSpringTypeId();
				if(ap.getRegionId() != 0)
					 storeSql +=" and regionId = "+ap.getRegionId();
				if(ap.getSteeringTypeId() != 0)
					 storeSql +=" and steeringTypeId = "+ap.getSteeringTypeId();
				if(ap.getSteeringSystemId() != 0)
					 storeSql +=" and steeringSystemId = "+ap.getSteeringSystemId();
				if(ap.getSubModelId() != 0)
					 storeSql +=" and subModelId = "+ap.getSubModelId();
				if(ap.getTransElecContolledId() != 0)
					 storeSql +=" and transElecContolledId = "+ap.getTransElecContolledId();
				if(ap.getTransmissionMfrId() != 0)
					 storeSql +=" and transmissionMfrId = "+ap.getTransmissionMfrId();
				if(ap.getTransmissionMfrCodeId() != 0)
					 storeSql +=" and transmissionMfrCodeId = "+ap.getTransmissionMfrCodeId();
				if(ap.getTransmissionBaseId() != 0)
					 storeSql +=" and transmissionBaseId = "+ap.getTransmissionBaseId();
				if(ap.getTransmissionControlTypeId() != 0)
					 storeSql +=" and transmissionControlTypeId = "+ap.getTransmissionControlTypeId();
				if(ap.getTransmissionNumSpeedsId() != 0)
					 storeSql +=" and transmissionNumSpeedsId = "+ap.getTransmissionNumSpeedsId();
				if(ap.getTransmissionTypeId() != 0)
					 storeSql +=" and transmissionTypeId = "+ap.getTransmissionTypeId();
				if(ap.getValvesPerEngineId() != 0)
					 storeSql +=" and valvesPerEngineId = "+ap.getValvesPerEngineId();
				if(ap.getVehicleTypeId() != 0)
					 storeSql +=" and vehicleTypeId = "+ap.getVehicleTypeId();
				if(ap.getWheelBaseId() != 0)
					 storeSql +=" and wheelBaseId = "+ap.getWheelBaseId();
				if(ap.getYearsFrom() != null)
					 storeSql +=" and yearsFrom = '"+ap.getYearsFrom()+"'";
				if(ap.getYearsTo() != null)
					 storeSql +=" and yearsTo = '"+ap.getYearsTo()+"'";
				
		  //  System.out.println("==>"+storeSql);
				rs = stmt.executeQuery(storeSql);
				
				//rs = con.prepareStatement(storeSql);
				if(!rs.next())
				{
							
				prSt = con.prepareStatement(insertApp);
				//prSt = con.prepareStatement(storeSql);
				prSt.setString(1, (ap.getAction()));
				prSt.setLong(2, (ap.getId()));
				prSt.setString(3, (ap.getRef()));
				prSt.setString(4, (ap.getValidate()));
				prSt.setLong(5, (ap.getAspirationId()));
				prSt.setString(6, (ap.getAssetAction()));
				prSt.setLong(7, (ap.getAssetId()));
				prSt.setString(8, (ap.getAssetRef()));
				prSt.setString(9, (ap.getAssetValidate()));
				prSt.setLong(10, (ap.getAssetItemOrder()));
				prSt.setString(11, (ap.getAssetItemRef()));
				prSt.setString(12, (ap.getAssetItemName()));
				prSt.setLong(13, (ap.getBaseVehicleId()));
				prSt.setLong(14, (ap.getBedLengthId()));
				prSt.setLong(15, (ap.getBedTypeId()));
				prSt.setLong(16, (ap.getBodyNumDoorsId()));
				prSt.setLong(17, (ap.getBodyTypeId()));
				prSt.setLong(18, (ap.getBrakeABSId()));
				prSt.setLong(19, (ap.getBrakeSystemId()));
				prSt.setLong(20, (ap.getCylinderHeadTypeId()));
				prSt.setLong(21, (ap.getDisplayOrder()));
				prSt.setLong(22, (ap.getDriveTypeId()));
				prSt.setLong(23, (ap.getEngineBaseId()));
				prSt.setLong(24, (ap.getEngineDesignationId()));
				prSt.setLong(25, (ap.getEngineMfrId()));
				prSt.setLong(26, (ap.getEngineVersionId()));
				prSt.setLong(27, (ap.getEngineVINId()));
				prSt.setLong(28, (ap.getFrontBrakeTypeId()));
				prSt.setLong(29, (ap.getFrontSpringTypeId()));
				prSt.setLong(30, (ap.getFuelDeliverySubTypeId()));
				prSt.setLong(31, (ap.getFuelDeliveryTypeId()));
				prSt.setLong(32, (ap.getFuelSystemControlTypeId()));
				prSt.setLong(33, (ap.getFuelSystemDesignId()));
				prSt.setLong(34, (ap.getFuelTypeId()));
				prSt.setLong(35, (ap.getIgnitionSystemTypeId()));
				prSt.setLong(36, (ap.getMakeId()));
				prSt.setString(37, (ap.getMfrLabel()));
				prSt.setLong(38, (ap.getMfrBodyCodeId()));
				prSt.setLong(39, (ap.getModelId()));
				prSt.setString(40, (ap.getNote()));
				prSt.setLong(41, (ap.getNoteId()));
				prSt.setString(42, (ap.getNoteLang()));
				prSt.setString(43, (ap.getPart()));
				prSt.setString(44, (ap.getPartBrandAAIAId()));
				prSt.setLong(45, (ap.getPartTypeId()));
				prSt.setLong(46, (ap.getPositionId()));
				prSt.setLong(47, (ap.getPowerOutputId()));
				prSt.setLong(48, (ap.getQualId()));
				prSt.setString(49, (ap.getqParamUom()));
				prSt.setString(50, (ap.getqParamValue()));
				prSt.setString(51, (ap.getqParamAltValue()));
				prSt.setString(52, (ap.getqParamAltUom()));
				prSt.setString(53, (ap.getqPText()));
				prSt.setLong(54, (ap.getQty()));
				prSt.setLong(55, (ap.getRearBrakeTypeId()));
				prSt.setLong(56, (ap.getRearSpringTypeId()));
				prSt.setLong(57, (ap.getRegionId()));
				prSt.setLong(58, (ap.getSteeringTypeId()));
				prSt.setLong(59, (ap.getSteeringSystemId()));
				prSt.setLong(60, (ap.getSubModelId()));
				prSt.setLong(61, (ap.getTransElecContolledId()));
				prSt.setLong(62, (ap.getTransmissionMfrId()));
				prSt.setLong(63, (ap.getTransmissionMfrCodeId()));
				prSt.setLong(64, (ap.getTransmissionBaseId()));
				prSt.setLong(65, (ap.getTransmissionControlTypeId()));
				prSt.setLong(66, (ap.getTransmissionNumSpeedsId()));
				prSt.setLong(67, (ap.getTransmissionTypeId()));
				prSt.setLong(68, (ap.getValvesPerEngineId()));
				prSt.setLong(69, (ap.getVehicleTypeId()));
				prSt.setLong(70, (ap.getWheelBaseId()));
				prSt.setString(71, (ap.getYearsFrom()));
				prSt.setString(72, (ap.getYearsTo()));
				prSt.setLong(73, s);
				prSt.setLong(74, s1);
				prSt.addBatch();
				prSt.executeBatch();
				out.println(ap.getId());
				prSt.close();
				insert++;
				System.out.println("Insert Record " +insert);
				}
				else{
					duplicate++;
					System.out.println("Duplicate record " +duplicate);
				
				}
			}
			//prSt.executeBatch();
			//prSt.close();
			System.out.println("Record added in App");
		
			con.commit();
			rs.close();
			prSt.close();
			System.out.println("Commited");
		} catch (Exception e) {
			//Mimimum acceptable free memory you think your app needs
			long minRunningMemory = (1024*1024);

			Runtime runtime = Runtime.getRuntime();

			if(runtime.freeMemory()<minRunningMemory)
			 System.gc();
			System.out.println(e);
			e.printStackTrace();
			
			
		}
		finally{
			try{ if(rs != null) rs.close(); 
				if(prSt != null) prSt.close(); if(con != null) con.close();
			 } catch(Exception ex){}
		}
	}
	
	/*
	public void warning(SAXParseException exception)
    throws SAXException {

    try {
        FileWriter fw = new FileWriter("error.log");
        BufferedWriter bw = new BufferedWriter(fw);
        bw.write("Warning: " + exception.getMessage( ) + "\n");
        bw.flush( );
        bw.close( );
        fw.close( );
    } catch (IOException e) {
        throw new SAXException("Could not write to log file", e);
    }*/
//}
	
public static void main(String[] args) {
		long startTime = System.nanoTime();
		ShiftHandler sh = new ShiftHandler();
		sh.runExample();
		long endTime = System.nanoTime();
		System.out.println("Took "+(endTime - startTime) + " ns");
	}

}

dinesh handler
import java.io.File;
import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;

import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

public class XML_Handler extends DefaultHandler {
   
	private static String url = "jdbc:oracle:thin:@192.168.2.68:1521:orcl";
	private static String userName = "test";
	private static String password = "test";
	public static Connection connect2 = null;

	private List<Header> headerList = null;
	private Header header = null;
	// private StringBuffer curCharValue = new StringBuffer(1024);
	private List<App> appList = null;
	private App app = null;

	private List<Footer> footerList = null;
	private Footer footer = null;

	// getter method for employee list
	public List<Header> getStudentList() {

		return headerList;
	}

	public List<App> getAppList() {
		return appList;
	}

	public List<Footer> getFooterList() {
		return footerList;
	}
	public static Connection connectionDriver() {
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			connect2 = DriverManager.getConnection(url, userName, password);
			System.out.println("Connected to Sql Server");
		} catch (Exception e) {

			e.printStackTrace();
		}
		return connect2;
	}

//----------------------------------------------------------- HEADER PARSING -----------------------------------------------------------	
	

	public static void passinHeader(List<Header> headerList) {
		Connection connect1 = connectionDriver();
		Connection conn=connectionDriver();
		PreparedStatement statement1 = null;
		
		/*PreparedStatement stmt1=null;
		long myid=0;
		
		try{
			String qury="select HEADER_INFO_SEQ.NEXTVAL from  HEADER_INFO   ";
			PreparedStatement prepar=conn.prepareStatement(qury);
			ResultSet rs=prepar.executeQuery();
			if(rs.next()){
				myid=rs.getLong(1);
			}
		}catch(Exception e1){
			e1.printStackTrace();
		}*/
		
		
		try {

			System.out.println("Established Conenction.....");
			String insertStudent = "INSERT INTO HEADER_DATA "
					+ "(Header_id,Company, SenderName, SenderPhone,TransferDate,BrandAAIAID,DocumentTitle,EffectiveDate,SubmissionType,MapperCompany,MapperContact,MapperPhone,MapperEmail,VcdbVersionDate,QdbVersionDate,PcdbVersionDate) VALUES "
					+ "(HEADER_DATA_SEQ.NEXTVAL,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?)";
		

			statement1 = connect1.prepareStatement(insertStudent);
			label11:for (Header header : headerList) {

				//System.out.println("Header Object=============="+header);
				
				try{
				statement1.setString(1, (header.getCompany()));
				statement1.setString(2, (header.getSenderName()));
				statement1.setString(3, (header.getSenderPhone()));
				statement1.setString(4, (header.getTransferDate()));
				statement1.setString(5, (header.getBrandAAIAID()));
				statement1.setString(6, (header.getDocumentTitle()));
				statement1.setString(7, (header.getEffectiveDate()));
				statement1.setString(8, (header.getSubmissionType()));
				statement1.setString(9, (header.getMapperCompany()));
				statement1.setString(10, (header.getMapperContact()));
				statement1.setString(11, (header.getMapperPhone()));
				statement1.setString(12, (header.getMapperEmail()));
				statement1.setString(13, (header.getVcdbVersionDate()));
				statement1.setString(14, (header.getQdbVersionDate()));
				statement1.setString(15, (header.getPcdbVersionDate()));
				

				System.out.println("excute uupdate start");
				statement1.execute();
				System.out.println("excute uupdate end");
				//connect1.commit();
				}catch(Exception e2){
					continue label11;
				}
			}// for close

		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				 statement1.close();
				 connect1.close();
			} catch (Exception e) {
				System.out.println("Exception in ps INSERT RECORD IN BATCH "+ e);
				// log("Cannot close result set", e);
			}
		}// finally ps close
	}

//----------------------------------------------------------- FOOTER PARSING -----------------------------------------------------------	
	
	
	public static void passinFOOTER(List<Footer> footerList) {
		
		Connection connect2 = connectionDriver();
		//Connection conn=connectionDriver();
		PreparedStatement statement2 = null;
		//PreparedStatement stmt1=null;
		/*long myid=0;
		
		try{
			String qury="select FOOTERTYPE_INFO_SEQ.NEXTVAL from FOOTERTYPE_INFO  ";
			PreparedStatement prepar=conn.prepareStatement(qury);
			ResultSet rs=prepar.executeQuery();
			if(rs.next()){
				myid=rs.getLong(1);
			}
		}catch(Exception e1){
			
		}*/
		
		
		try {
			

			System.out.println("Established Conenction.....");
			String insertStudent = "INSERT INTO FOOTERTYPE_DATA "
									+ "(Footer_id,RecordCount) VALUES"
									+ "(FOOTERTYPE_DATA_SEQ.NEXTVAL,?)";
			statement2 = connect2.prepareStatement(insertStudent);
				for (Footer footer : footerList) {
	                    //String s1=""+myid+footer.getRecordCount();
						
	
						statement2.setString(1, (footer.getRecordCount()));
	                    //statement2.setString(2, s1);
						statement2.execute();
				}
			//connect2.commit();
			
			System.out.println("end prgram");
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				 statement2.close();
				 connect2.close();
			} catch (Exception e) {
				System.out.println("Exception in ps INSERT RECORD IN BATCH "+ e);
				// log("Cannot close result set", e);
			}
		}// finally ps close
	}
	

//----------------------------------------------------------- APPTYPE PARSING -----------------------------------------------------------	
	
	public static void passinAPPTYPE(List<App> appList) {
		
		Connection hdconnect3 = connectionDriver();
		Connection ftconnect4 = connectionDriver();
		Connection appconnect5 = connectionDriver();
		Connection rsultconnect=connectionDriver();
		
		PreparedStatement statement3 = null;
		Statement headstmt3 = null;
		Statement footerstmt4 =null;
		PreparedStatement getAppdata=null;
		ResultSet rs=null;
		int s = 0;
		int head = 0;
		int s1 = 0;
		int foot = 0;
		ResultSet rs1=null;
		ResultSet selectStudent=null;
		try {
			
			//PreparedStatement statement3 = connect2.prepareStatement("select MAX(HEADER_ID) from HEADER_INFO");
			headstmt3 = hdconnect3.createStatement();
			 
			
			
			 rs = headstmt3.executeQuery("select MAX(HEADER_ID) from HEADER_DATA");
			 if(rs!=null){
			while(rs.next()){
			head = rs.getInt(1);
			}
			 }
			System.out.println("header id======================"+head);
			s = head;
			
			//int s = statement3.execute();			
			System.out.println("HEADER_ID="+s);
		}
		catch(Exception e2){
			e2.printStackTrace();
		}
		finally{
			try{
				headstmt3.close();
				hdconnect3.close();
				rs.close();
			}catch(Exception e1){
				
			}
		}
			//s++;
			
             // connect2.commit();
			
		//---------------------------------------------------------------------------------------------------------------------
			
			//PreparedStatement stmt = connect3.prepareStatement("select MAX(FOOTER_ID) from FOOTERTYPE_INFO");
		try{
			footerstmt4 = ftconnect4.createStatement();
			
		//	int s1 = 0;
		//	int foot = 0;
			
			 rs1= footerstmt4.executeQuery("select MAX(FOOTER_ID) from FOOTERTYPE_DATA");
			 if(rs1!=null){
			while(rs1.next()){
			foot=rs1.getInt(1);	
			}
			 }
			s1 = foot;
			
			//int s1 = stmt.executeUpdate();
			System.out.println("FOOTER_ID="+s1);
			
			//getAppdata=rsultconnect.createStatement();
			//s1++;
            //connect3.commit();
			System.out.println("Established Conenction.....");
		}
		catch(Exception e2){
			e2.printStackTrace();
		}
			finally{
				try{
					footerstmt4.close();
					ftconnect4.close();
					rs1.close();
				}catch(Exception e1){
					e1.printStackTrace();
				}
			}
			
           try{
			
			//appconnect5.setAutoCommit(false);
			//getAppdata=
			//int batchSize = 500;
			//int count = 0;
			 String insertStudent = "INSERT INTO APPTYPE_DATA "
			+ "VALUES"
			+ "(APPTYPE_DATA_SEQ.NEXTVAL,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?, "
			+ "?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?)";
        statement3 = appconnect5.prepareStatement(insertStudent);
			int count2=0;
				label1:for (App app : appList) {
					try {
		                statement3.setInt(1, s);
						statement3.setLong(2, (app.getId()));
						statement3.setString(3, (app.getAction()));
						statement3.setString(4, (app.getValidate()));
						statement3.setString(5, (app.getBaseVehicle()));
						statement3.setLong(6, (app.getBaseVehicleId()));
						statement3.setString(7, (app.getEngineBase()));
						statement3.setLong(8, (app.getEngineBaseId()));
						statement3.setString(9, (app.getEngineDesignation()));
						statement3.setLong(10, (app.getEngineDesignationId()));
						statement3.setLong(11, (app.getFuelDeliverySubType()));
						statement3.setString(12, (app.getNote()));
						statement3.setString(13, (app.getQty()));
						statement3.setString(14, (app.getPartType()));
						statement3.setLong(15, (app.getPartTypeId()));
						statement3.setLong(16, (app.getPosition()));
						statement3.setString(17, (app.getPart()));
						statement3.setString(18, (app.getAspiration()));
						statement3.setLong(19, (app.getAspirationId()));
						statement3.setLong(20, (app.getBedType()));
						statement3.setLong(21, (app.getBodyNumDoors()));
						statement3.setLong(22, (app.getBodyType()));
						statement3.setLong(23, (app.getBrakeABS()));
						statement3.setLong(24, (app.getCylinderHeadType()));
						statement3.setString(25, (app.getDisplayOrder()));
						statement3.setLong(26, (app.getDriveType()));
						statement3.setLong(27, (app.getEngineMfr()));
						statement3.setString(28, (app.getEngineVersion()));
						statement3.setLong(29, (app.getEngineVersionId()));
						statement3.setString(30, (app.getEngineVIN()));
						statement3.setLong(31, (app.getEngineVINId()));
						statement3.setLong(32, (app.getFrontBrakeType()));
						statement3.setLong(33, (app.getFrontSpringType()));
						statement3.setLong(34, (app.getFuelDeliveryType()));
						statement3.setLong(35, (app.getFuelSystemControlType()));
						statement3.setLong(36, (app.getFuelSystemDesign()));
						statement3.setLong(37, (app.getFuelType()));
						statement3.setLong(38, (app.getIgnitionSystemType()));
						statement3.setLong(39, (app.getMake()));
						statement3.setString(40, (app.getMfrLabel()));
						statement3.setLong(41, (app.getModel()));
						statement3.setLong(42, (app.getPowerOutput()));
						statement3.setLong(43, (app.getQual()));
						statement3.setLong(44, (app.getRearBrakeType()));
						statement3.setLong(45, (app.getRearSpringType()));
						statement3.setLong(46, (app.getRegion()));
						statement3.setLong(47, (app.getSteeringType()));
						statement3.setLong(48, (app.getSteeringSystem()));
						statement3.setString(49, (app.getSubModel()));
						statement3.setLong(50, (app.getSubModelId()));
						statement3.setLong(51, (app.getTransElecControlled()));
						statement3.setLong(52, (app.getTransmissionMfr()));
						statement3.setLong(53, (app.getTransmissionMfrCode()));
						statement3.setLong(54, (app.getTransmissionBase()));
						statement3.setLong(55, (app.getTransmissionControlType()));
						statement3.setLong(56, (app.getTransmissionNumSpeeds()));
						statement3.setLong(57, (app.getTransmissionType()));
						statement3.setLong(58, (app.getValvesPerEngine()));
						statement3.setLong(59, (app.getVehicleType()));
						statement3.setLong(60, (app.getWheelBase()));
						statement3.setString(61, (app.getFromYears()));
						statement3.setString(62, (app.getToYears()));
						statement3.setLong(63, s1);
						statement3.setString(64, app.toString());
						statement3.execute();
                   
						 
								
					       }catch(Exception e1){
					    	   e1.printStackTrace();
					    	   System.out.println("Error occured"+count2++);
					    	   continue label1;
					    	   					       }
					
                 }// for loop
				
				//----------------------------------------------------------------------------------------------
					
				//----------------------------------------------------------------------------------------------					
				//connect5.commit();			

			System.out.println("end prgram");
			} catch (Exception e) {
				e.printStackTrace();
	
			} finally {				
				// connect4.commit();
				try {
					statement3.close();
					appconnect5.close();					
				} catch (Exception e) {
					System.out.println("Exception in ps INSERT RECORD IN BATCH "+ e);
					// log("Cannot close result set", e);
				}
			}// finally ps close
			
		}



	public static void main(String args[]) throws ParserConfigurationException,
			SAXException, IOException {
		// SAXParserFactory saxParserFactory = SAXParserFactory.newInstance();
		long starttime=System.currentTimeMillis()/1000;
		XML_Handler handler = new XML_Handler();
		handler.SAX_Parser();
		handler.SAX_Parser2();
		handler.SAX_Parser1();

		System.out.println("end of data inserted successfully");
		long endtime=System.currentTimeMillis();
		long totaltime=endtime-starttime/1000;
		System.out.println("data inserted in table in seconds"+totaltime);
	}

	public void SAX_Parser() {
		SAXParserFactory saxParserFactory = SAXParserFactory.newInstance();
		try {
			SAXParser saxParser = saxParserFactory.newSAXParser();
			XML_Handler handler = new XML_Handler();
			

			
			  saxParser .parse( new File("D:\\Mamta\\New folder\\xml\\Sealed_Power_ACES_FULL_12-28-2014.xml"), handler);
			 

			
			List<Header> headerList = handler.getStudentList();
			// for (Header header : headerList)
			// System.out.println(header);
			passinHeader(headerList);

		} catch (Exception e) {
			e.printStackTrace();

		}
	}

	public void SAX_Parser1() {
		SAXParserFactory saxParserFactory = SAXParserFactory.newInstance();
		try {
			SAXParser saxParser = saxParserFactory.newSAXParser();
			XML_Handler handler = new XML_Handler();
			
		

			
			 saxParser .parse( new File(
			  "D:\\Mamta\\New folder\\xml\\Sealed_Power_ACES_FULL_12-28-2014.xml"
			  ), handler);
			 

			/*saxParser
					.parse(
							new File(
									"C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_VictorReinz_Gaskets.xml"),
							handler);*/

			
			/*  saxParser .parse( new File(
			  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_MAHLEOriginal_Thermostats.xml"
			  ), handler);
			 */

			
		/*	  saxParser .parse( new File(
			  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_MAHLEOriginal_Turbocharger.xml"
			  ), handler);*/
			 

			/* saxParser .parse( new File(
					  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_XML_v3_for_Opticat_20140706175233_Clevite-Bearings.xml"
					  ), handler);
			*/
		/*	saxParser .parse( new File(
					  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_XML_v3_for_Opticat_20140706175231_MAHLEOriginal-Pistons.xml"
					  ), handler);*/
			
		/*	saxParser .parse( new File(
					  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_MAHLEOriginal_Turbochargers.xml"
					  ), handler);*/
			// Get Header list
			List<App> appList = handler.getAppList();
			// for (App app : appList)
			// System.out.println("APPLICATION OBJECT="+app);
			System.out.println(appList.size());
			passinAPPTYPE(appList);
			// return appList;

		} catch (Exception e) {
			e.printStackTrace();
			// return null;
		}
	}

	public void SAX_Parser2() {
		SAXParserFactory saxParserFactory = SAXParserFactory.newInstance();
		try {
			SAXParser saxParser = saxParserFactory.newSAXParser();
			XML_Handler handler = new XML_Handler();
			
			 saxParser .parse( new File(
			  "D:\\Mamta\\New folder\\xml\\Sealed_Power_ACES_FULL_12-28-2014.xml"
			 ), handler);
			 
			
		/*	 saxParser .parse( new File(
			  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_MAHLEOriginal_Filters_20150204.xml"
			  ), handler);
			 */

			
			//  saxParser .parse( new File(
			 // "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_MAHLEOriginal_PistonRings.xml"
			//  ), handler);
			 
/*
			saxParser
					.parse(
							new File(
									"C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_VictorReinz_Gaskets.xml"),
							handler);*/

			
		/*	  saxParser .parse( new File(
			  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_MAHLEOriginal_Thermostats.xml"
			  ), handler);*/
			 

			
		/*	  saxParser .parse( new File(
			  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_MAHLEOriginal_Turbocharger.xml"
			  ), handler);
			 */
			
		/*	 saxParser .parse( new File(
					  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_XML_v3_for_Opticat_20140706175233_Clevite-Bearings.xml"
					  ), handler);*/
					 
		/*	saxParser .parse( new File(
					  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_XML_v3_for_Opticat_20140706175231_MAHLEOriginal-Pistons.xml"
					  ), handler);*/
			
		/*	saxParser .parse( new File(
					  "C:\\Users\\PC-35\\Workspaces\\MyEclipse 8.x\\SAX_PARSER_ORIGINAL\\WebRoot\\ACES_MAHLEOriginal_Turbochargers.xml"
					  ), handler);*/

			// Get Header list
			List<Footer> footerList = handler.getFooterList();
			// for (Footer footer : footerList)
			// System.out.println("APPLICATION OBJECT="+footer);
			passinFOOTER(footerList);

		} catch (Exception e) {
			e.printStackTrace();

		}
	}

	// List to hold Header object

	/* data for HeaderInfo Table */
	boolean bCompany = false;
	boolean bSenderName = false;
	boolean bSenderPhone = false;
	boolean bTransferDate = false;
	boolean bBrandAAIAID = false;
	boolean bDocumentTitle = false;
	boolean bEffectiveDate = false;
	boolean bSubmissionType = false;
	boolean bMapperCompany = false;
	boolean bMapperContact = false;
	boolean bMapperPhone = false;
	boolean bMapperEmail = false;
	boolean bVcdbVersionDate = false;
	boolean bQdbVersionDate = false;
	boolean bPcdbVersionDate = false;
	boolean bApprovedFor = false;
	boolean bDocFormNumber = false;
	boolean bMapperPhoneExt = false;
	boolean bSenderPhoneExt = false;

	/* data for AppType_info table */
	boolean bBaseVehicle = false;
	boolean bBaseVehicleId=false;
	boolean bEngineBase = false;
	boolean bEngineBaseId=false;
	boolean bEngineDesignation = false;
	boolean bEngineDesignationid=false;
	boolean bFuelDeliverySubType = false;
	boolean bNote = false;
	boolean bQty = false;
	boolean bPartType = false;
	boolean bPartTypeId=false;
	boolean bPosition = false;
	boolean bPart = false;

	boolean bAspiration = false;
	boolean bAspirationId=false;
	boolean bBedType = false;
	boolean bBodyNumDoors = false;
	boolean bBodyType = false;
	boolean bBrakeABS = false;
	boolean bCylinderHeadType = false;
	boolean bDisplayOrder = false;
	boolean bDriveType = false;
	boolean bEngineMfr = false;
	boolean bEngineVersion = false;
	boolean bEngineVersionId=false;
	boolean bEngineVIN = false;
	boolean bEgnineVINId=false;
	boolean bFrontBrakeType = false;
	boolean bFrontSpringType = false;
	boolean bFuelDeliveryType = false;
	boolean bFuelSystemControlType = false;
	boolean bFuelSystemDesign = false;
	boolean bFuelType = false;
	boolean bIgnitionSystemType = false;
	boolean bMake = false;
	boolean bMfrLabel = false;
	boolean bModel = false;
	boolean bPowerOutput = false;
	boolean bQual = false;
	boolean bRearBrakeType = false;
	boolean bRearSpringType = false;
	boolean bRegion = false;
	boolean bSteeringType = false;
	boolean bSteeringSystem = false;
	boolean bSubModel = false;
	boolean bSubModelId=false;
	boolean bTransElecControlled = false;
	boolean bTransmissionMfr = false;
	boolean bTransmissionMfrCode = false;
	boolean bTransmissionBase = false;
	boolean bTransmissionControlType = false;
	boolean bTransmissionNumSpeeds = false;
	boolean bTransmissionType = false;
	boolean bValvesPerEngine = false;
	boolean bVehicleType = false;
	boolean bWheelBase = false;

	boolean bfrom = false;
	boolean bto = false;

	/* data for FooterType_info table */
	boolean bRecordCount = false;

	@Override
	public void startElement(String uri, String localName, String qName,
			Attributes attributes) throws SAXException {

		if (qName.equalsIgnoreCase("Header")) {

			// String id = attributes.getValue("id");
			// initialize S object and set id attribute
			header = new Header();
			// header.setId(Integer.parseInt(id));

			if (headerList == null)
				headerList = new ArrayList<Header>();
		} else if (qName.equalsIgnoreCase("Company")) {
			bCompany = true;
		} else if (qName.equalsIgnoreCase("SenderName")) {
			bSenderName = true;
		} else if (qName.equalsIgnoreCase("SenderPhone")) {
			bSenderPhone = true;
		} else if (qName.equalsIgnoreCase("TransferDate")) {
			bTransferDate = true;
		}

		else if (qName.equalsIgnoreCase("BrandAAIAID")) {
			bBrandAAIAID = true;
		} else if (qName.equalsIgnoreCase("DocumentTitle")) {
			bDocumentTitle = true;
		} else if (qName.equalsIgnoreCase("EffectiveDate")) {
			bEffectiveDate = true;
		}

		else if (qName.equalsIgnoreCase("SubmissionType")) {
			bSubmissionType = true;
		} else if (qName.equalsIgnoreCase("MapperCompany")) {
			bMapperCompany = true;
		} else if (qName.equalsIgnoreCase("MapperContact")) {
			bMapperContact = true;
		} else if (qName.equalsIgnoreCase("MapperPhone")) {
			bMapperPhone = true;
		} else if (qName.equalsIgnoreCase("MapperEmail")) {
			bMapperEmail = true;
		} else if (qName.equalsIgnoreCase("VcdbVersionDate")) {
			bVcdbVersionDate = true;
		} else if (qName.equalsIgnoreCase("QdbVersionDate")) {
			bQdbVersionDate = true;
		} else if (qName.equalsIgnoreCase("PcdbVersionDate")) {
			bPcdbVersionDate = true;
		} else if (qName.equalsIgnoreCase("ApprovedFor")) {
			bApprovedFor = true;
		}

		else if (qName.equalsIgnoreCase("DocFormNumber")) {
			bDocFormNumber = true;
		}

		else if (qName.equalsIgnoreCase("MapperPhoneExt")) {
			bMapperPhoneExt = true;
		}

		else if (qName.equalsIgnoreCase("SenderPhoneExt")) {
			bSenderPhoneExt = true;
		}

		else if (qName.equalsIgnoreCase("App")) {

			String id = attributes.getValue("id");
			String action = attributes.getValue("action");
			String validate = attributes.getValue("validate");

			// initialize S object and set id attribute
			app = new App();
			app.setId(Integer.parseInt(id));
			app.setAction(action);
			app.setValidate(validate);
			if (appList == null)
				appList = new ArrayList<App>();
		} else if (qName.equalsIgnoreCase("BaseVehicle")) {
			String id = attributes.getValue("id");
			// app=new App();
			
			app.setBaseVehicleId(Long.parseLong(id));
			bBaseVehicle=true;
			// if(appList==null)
			// appList=new ArrayList<App>();
			// System.out.println("BaseVehicleId" + id);
		}

		else if (qName.equalsIgnoreCase("EngineBase")) {
			String id = attributes.getValue("id");
			app.setEngineBaseId(Long.parseLong(id));
			bEngineBase=true;
			// System.out.println("EngineBaseId" + id);
		} else if (qName.equalsIgnoreCase("EngineDesignation")) {
			String id = attributes.getValue("id");
			app.setEngineDesignationId(Long.parseLong(id));
			bEngineDesignation=true;
			// System.out.println("EngineDesignationId" + id);
		} else if (qName.equalsIgnoreCase("FuelDeliverySubType")) {
			String id = attributes.getValue("id");
			app.setFuelDeliverySubType(Long.parseLong(id));
			// System.out.println("FuelDeliverySubType" + id);

		} else if (qName.equals("Note")) {
			// app=new App();
			// app.setNote();
			// String note1=qName.equals("Note");

			bNote = true;
		} else if (qName.equalsIgnoreCase("Qty")) {
			// app.setQty("Qty");
			bQty = true;
		} else if (qName.equalsIgnoreCase("PartType")) {
			String id = attributes.getValue("id");
			app.setPartTypeId(Long.parseLong(id));
			System.out.println("PartTypeId" + id);
			bPartType=true;
		} else if (qName.equalsIgnoreCase("Position")) {
			String id = attributes.getValue("id");
			app.setPosition(Long.parseLong(id));
			System.out.println("Position" + id);

		} else if (qName.equalsIgnoreCase("Part")) {
			// app.setPart("Part");
			bPart = true;
		}

		else if (qName.equalsIgnoreCase("Aspiration")) {
			String id = attributes.getValue("id");
			app.setAspirationId(Long.parseLong(id));
			bAspiration=true;
		}

		else if (qName.equalsIgnoreCase("BedType")) {
			String id = attributes.getValue("id");
			app.setBedType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("BodyNumDoors")) {
			String id = attributes.getValue("id");
			app.setBodyNumDoors(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("BodyType")) {
			String id = attributes.getValue("id");
			app.setBodyType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("BrakeABS")) {
			String id = attributes.getValue("id");
			app.setBrakeABS(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("CylinderHeadType")) {
			String id = attributes.getValue("id");
			app.setCylinderHeadType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("DisplayOrder")) {
			// String id=attributes.getValue("id");
			bDisplayOrder = true;
		}

		else if (qName.equalsIgnoreCase("DriveType")) {
			String id = attributes.getValue("id");
			app.setDriveType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("EngineMfr")) {
			String id = attributes.getValue("id");
			app.setEngineMfr(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("EngineVersion")) {
			String id = attributes.getValue("id");
			app.setEngineVersionId(Long.parseLong(id));
			bEngineVersion=true;
		}

		else if (qName.equalsIgnoreCase("EngineVIN")) {
			String id = attributes.getValue("id");
			app.setEngineVINId(Long.parseLong(id));
			bEngineVIN=true;
		}

		else if (qName.equalsIgnoreCase("FrontBrakeType")) {
			String id = attributes.getValue("id");
			app.setFrontBrakeType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("FrontSpringType")) {
			String id = attributes.getValue("id");
			app.setFrontSpringType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("FuelDeliveryType")) {
			String id = attributes.getValue("id");
			app.setFuelType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("FuelSystemControlType")) {
			String id = attributes.getValue("id");
			app.setFuelSystemControlType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("FuelSystemDesign")) {
			String id = attributes.getValue("id");
			app.setFuelSystemDesign(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("FuelType")) {
			String id = attributes.getValue("id");
			app.setFuelType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("IgnitionSystemType")) {
			String id = attributes.getValue("id");
			app.setIgnitionSystemType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("Make")) {
			String id = attributes.getValue("id");
			app.setMake(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("MfrLabel")) {
			bMfrLabel = true;
		}

		else if (qName.equalsIgnoreCase("Model")) {
			String id = attributes.getValue("id");
			app.setModel(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("PowerOutput")) {
			String id = attributes.getValue("id");
			app.setPowerOutput(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("Qual")) {
			String id = attributes.getValue("id");
			app.setQual(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("RearBrakeType")) {
			String id = attributes.getValue("id");
			app.setRearBrakeType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("RearSpringType")) {
			String id = attributes.getValue("id");
			app.setRearSpringType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("Region")) {
			String id = attributes.getValue("id");
			app.setRegion(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("SteeringType")) {
			String id = attributes.getValue("id");
			app.setSteeringType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("SteeringSystem")) {
			String id = attributes.getValue("id");
			app.setSteeringSystem(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("SubModel")) {
			String id = attributes.getValue("id");
			app.setSubModelId(Long.parseLong(id));
			bSubModel=true;
		}

		else if (qName.equalsIgnoreCase("TransElecControlled")) {
			String id = attributes.getValue("id");
			app.setTransElecControlled(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("TransmissionMfr")) {
			String id = attributes.getValue("id");
			app.setTransmissionMfr(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("TransmissionMfrCode")) {
			String id = attributes.getValue("id");
			app.setTransmissionMfrCode(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("TransmissionBase")) {
			String id = attributes.getValue("id");
			app.setTransmissionBase(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("TransmissionControlType")) {
			String id = attributes.getValue("id");
			app.setTransmissionControlType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("TransmissionNumSpeeds")) {
			String id = attributes.getValue("id");
			app.setTransmissionNumSpeeds(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("TransmissionType")) {
			String id = attributes.getValue("id");
			app.setTransmissionType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("ValvesPerEngine")) {
			String id = attributes.getValue("id");
			app.setValvesPerEngine(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("VehicleType")) {
			String id = attributes.getValue("id");
			app.setVehicleType(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("WheelBase")) {
			String id = attributes.getValue("id");
			app.setWheelBase(Long.parseLong(id));
		}

		else if (qName.equalsIgnoreCase("Years")) {
			String from = attributes.getValue("from");
			app.setFromYears(from);
			String to = attributes.getValue("to");
			app.setToYears(to);
		}

		else if (qName.equalsIgnoreCase("Footer")) {
			footer = new Footer();
			// header.setId(Integer.parseInt(id));

			if (footerList == null)
				footerList = new ArrayList<Footer>();
		} else if (qName.equalsIgnoreCase("RecordCount")) {
			bRecordCount = true;
		}

	}

	@Override
	public void endElement(String uri, String localName, String qName)
			throws SAXException {
		if (qName.equalsIgnoreCase("Header")) {
			// add Header object to list
			headerList.add(header);
		}
		if (qName.equalsIgnoreCase("App")) {
			// add App object to list
			appList.add(app);
			if(appList.size() ==500){
				passinAPPTYPE(appList);
				//appList.clear();
				appList = null;
			}

		}
		if (qName.equalsIgnoreCase("Footer")) {
			// add Footer object to list
			footerList.add(footer);
			
		}

	}

	@Override
	public void characters(char ch[], int start, int length)
			throws SAXException {

		if (bCompany) {
			// age element, set age
			header.setCompany(new String(ch, start, length));
			bCompany = false;
		} else if (bSenderName) {
			header.setSenderName(new String(ch, start, length));
			bSenderName = false;

		} else if (bSenderPhone) {
			header.setSenderPhone(new String(ch, start, length));
			bSenderPhone = false;
		} else if (bTransferDate) {
			header.setTransferDate(new String(ch, start, length));
			bTransferDate = false;
		}

		else if (bBrandAAIAID) {
			header.setBrandAAIAID(new String(ch, start, length));
			bBrandAAIAID = false;
		} else if (bDocumentTitle) {
			header.setDocumentTitle(new String(ch, start, length));
			bDocumentTitle = false;
		} else if (bEffectiveDate) {
			header.setEffectiveDate(new String(ch, start, length));
			bEffectiveDate = false;
		}

		else if (bSubmissionType) {
			header.setSubmissionType(new String(ch, start, length));
			bSubmissionType = false;
		} else if (bMapperCompany) {
			header.setMapperCompany(new String(ch, start, length));
			bMapperCompany = false;
		} else if (bMapperContact) {
			header.setMapperContact(new String(ch, start, length));
			bMapperContact = false;
		} else if (bMapperPhone) {
			header.setMapperPhone(new String(ch, start, length));
			bMapperPhone = false;
		} else if (bMapperEmail) {
			header.setMapperEmail(new String(ch, start, length));
			bMapperEmail = false;
		} else if (bVcdbVersionDate) {
			header.setVcdbVersionDate(new String(ch, start, length));
			bVcdbVersionDate = false;
		} else if (bQdbVersionDate) {
			header.setQdbVersionDate(new String(ch, start, length));
			bQdbVersionDate = false;
		} else if (bPcdbVersionDate) {
			header.setPcdbVersionDate(new String(ch, start, length));
			bPcdbVersionDate = false;
		}

		
		/* method of App record */
		else if (bBaseVehicle) {
			app.setBaseVehicle(new String(ch, start, length));
			bBaseVehicle = false;

		} else if (bEngineBase) {
			app.setEngineBase(new String(ch, start, length));
			bEngineBase = false;

		}

		else if (bEngineDesignation) {
			app.setEngineDesignation( new String(ch, start, length));
			bEngineDesignation = false;

		} else if (bFuelDeliverySubType) {
			System.out.println("FuelDeliverySubType: "
					+ new String(ch, start, length));
			bFuelDeliverySubType = false;

		} else if (bNote) {
			if (bNote)
				app.setNote(new String(ch, start, length));
			bNote = false;

		} else if (bQty) {
			app.setQty(new String(ch, start, length));
			bQty = false;

		} else if (bPartType) {
			System.out.println("PartType: " + new String(ch, start, length));
			bPartType = false;

		} else if (bPosition) {
			System.out.println("Position: " + new String(ch, start, length));
			bPosition = false;

		} else if (bPart) {
			System.out.println("Part : "+new String(ch, start, length));
			bPart = false;

		}

		else if (bAspiration) {
			app.setAspiration(new String(ch, start, length));
			bAspiration = false;

		}

		else if (bBedType) {
			System.out.println("BedType :" + new String(ch, start, length));
			bBedType = false;

		}

		else if (bBodyNumDoors) {
			System.out
					.println("BodyNumDoors :" + new String(ch, start, length));
			bBodyNumDoors = false;
		}

		else if (bBodyType) {
			System.out.println("BodyType :" + new String(ch, start, length));
			bBodyType = false;
		}

		else if (bBrakeABS) {
			System.out.println("BrakeABS :" + new String(ch, start, length));
			bBrakeABS = false;

		}

		else if (bCylinderHeadType) {
			System.out.println("CylinderHeadType :"
					+ new String(ch, start, length));
			bCylinderHeadType = false;

		}

		else if (bDisplayOrder) {
			app.setDisplayOrder(new String(ch, start, length));
			bDisplayOrder = false;

		}

		else if (bDriveType) {
			System.out.println("DriveType :" + new String(ch, start, length));
			bDriveType = false;

		}

		else if (bEngineMfr) {
			System.out.println("EngineMfr :" + new String(ch, start, length));
			bEngineMfr = false;

		}

		else if (bEngineVersion) {
			app.setEngineVersion( new String(ch, start, length));
			bEngineVersion = false;

		}

		else if (bEngineVIN) {
			app.setEngineVIN( new String(ch, start, length));
			bEngineVIN = false;

		}

		else if (bFrontBrakeType) {
			System.out.println("FrontBrakeType :"
					+ new String(ch, start, length));
			bFrontBrakeType = false;

		}

		else if (bFrontSpringType) {
			System.out.println("FrontSpringType :"
					+ new String(ch, start, length));
			bFrontSpringType = false;

		}

		else if (bFuelDeliveryType) {
			System.out.println("FuelDeliveryType :"
					+ new String(ch, start, length));
			bFuelDeliveryType = false;
		}

		else if (bFuelSystemControlType) {
			System.out.println("FuelSystemControlType :"
					+ new String(ch, start, length));
			bFuelSystemControlType = false;

		}

		else if (bFuelSystemDesign) {
			System.out.println("FuelSystemDesign :"
					+ new String(ch, start, length));
			bFuelSystemDesign = false;

		}

		else if (bFuelType) {
			System.out.println("FuelType :" + new String(ch, start, length));
			bFuelType = false;

		}

		else if (bIgnitionSystemType) {
			System.out.println("IgnitionSystemType :"
					+ new String(ch, start, length));
			bIgnitionSystemType = false;

		}

		else if (bMake) {
			System.out.println("Make :" + new String(ch, start, length));
			bMake = false;

		}

		else if (bMfrLabel) {
			System.out.println("MfrLabel :" + new String(ch, start, length));
			bMfrLabel = false;

		}

		else if (bModel) {
			System.out.println("Model :" + new String(ch, start, length));
			bModel = false;

		}

		else if (bPowerOutput) {
			System.out.println("PowerOutput :" + new String(ch, start, length));
			bPowerOutput = false;

		}

		else if (bQual) {
			System.out.println("Qual :" + new String(ch, start, length));
			bQual = false;

		}

		else if (bRearBrakeType) {
			System.out.println("RearBrakeType :"
					+ new String(ch, start, length));
			bRearBrakeType = false;

		}

		else if (bRearSpringType) {
			System.out.println("RearSpringType :"
					+ new String(ch, start, length));
			bRearSpringType = false;

		}

		else if (bRegion) {
			System.out.println("Region :" + new String(ch, start, length));
			bRegion = false;

		}

		else if (bSteeringType) {
			System.out
					.println("SteeringType :" + new String(ch, start, length));
			bSteeringType = false;

		}

		else if (bSteeringSystem) {
			System.out.println("SteeringSystem :"
					+ new String(ch, start, length));
			bSteeringSystem = false;

		}

		else if (bSubModel) {
			app.setSubModel(new String(ch, start, length));
			bSubModel = false;

		}

		else if (bTransElecControlled) {
			System.out.println("TransElecControlled :"
					+ new String(ch, start, length));
			bTransElecControlled = false;

		}

		else if (bTransmissionMfr) {
			System.out.println("TransmissionMfr :"
					+ new String(ch, start, length));
			bTransmissionMfr = false;

		} else if (bTransmissionMfrCode) {
			System.out.println("TransmissionMfrCode :"
					+ new String(ch, start, length));
			bTransmissionMfrCode = false;

		}

		else if (bTransmissionBase) {
			System.out.println("TransmissionBase :"
					+ new String(ch, start, length));
			bTransmissionBase = false;

		}

		else if (bTransmissionControlType) {
			System.out.println("TransmissionControlType :"
					+ new String(ch, start, length));
			bTransmissionControlType = false;

		}

		else if (bTransmissionNumSpeeds) {
			System.out.println("TransmissionNumSpeeds :"
					+ new String(ch, start, length));
			bTransmissionNumSpeeds = false;

		}

		else if (bTransmissionType) {
			System.out.println("TransmissionType :"
					+ new String(ch, start, length));
			bTransmissionType = false;

		}

		else if (bValvesPerEngine) {
			System.out.println("ValvesPerEngine :"
					+ new String(ch, start, length));
			bValvesPerEngine = false;

		}

		else if (bVehicleType) {
			System.out.println("VehicleType :" + new String(ch, start, length));
			bVehicleType = false;

		}

		else if (bWheelBase) {
			System.out.println("WheelBase :" + new String(ch, start, length));
			bWheelBase = false;

		}

		else if (bfrom) {
			System.out.println(" from:" + new String(ch, start, length));
			bfrom = false;

		} else if (bto) {
			System.out.println("to:" + new String(ch, start, length));
			bto = false;
		} else if (bRecordCount) {
			footer.setRecordCount(new String(ch, start, length));
			bRecordCount = false;
		}

	}
}


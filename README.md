# BasicUploading
## Java 8
  
    
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.*;
    import org.springframework.web.multipart.MultipartFile;
    import java.io.File;

    @Controller // This means that this class is a Controller
    @RequestMapping(path="/users") // This means URL's start with /userAPI (after Application path)
    @CrossOrigin(origins = "http://localhost:3000")
    public class UserController {

        public static final Logger logger = LoggerFactory.getLogger(UserController.class);
        @RequestMapping(method = RequestMethod.POST, value = "/upload", consumes = "multipart/form-data")
        public @ResponseBody String upload(MultipartFile file) {

            if (file == null) return "No file selected";

            try {
                String path = System.getProperty("user.dir") + "/uploads/"+file.getOriginalFilename();
                logger.info("File: " + path);

                File destFile = new File(path);

                // create directory if not exist
                createParentDir(destFile);

                file.transferTo(new File(path));
                logger.info("File has been transfered to " + System.getProperty("user.dir") + "/uploads/");

            } catch (Exception e) {
                logger.error(e.getMessage());
                return "Not uploaded";
            }
            return "Uploaded";
        }

        private void createParentDir(File destFile) {
            File parentFile = destFile.getParentFile();
            if (parentFile != null && !parentFile.exists()) {
                if (!parentFile.exists()) {
                    parentFile.mkdirs();
                }
            }
        }
     }

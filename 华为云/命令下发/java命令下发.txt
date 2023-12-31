import com.huaweicloud.sdk.core.auth.AbstractCredentials;
import com.huaweicloud.sdk.core.auth.BasicCredentials;
import com.huaweicloud.sdk.core.auth.ICredential;
import com.huaweicloud.sdk.core.exception.ConnectionException;
import com.huaweicloud.sdk.core.exception.RequestTimeoutException;
import com.huaweicloud.sdk.core.exception.ServiceResponseException;
import com.huaweicloud.sdk.core.region.Region;
import com.huaweicloud.sdk.iotda.v5.IoTDAClient;
import com.huaweicloud.sdk.iotda.v5.model.CreateCommandRequest;
import com.huaweicloud.sdk.iotda.v5.model.CreateCommandResponse;
import com.huaweicloud.sdk.iotda.v5.model.DeviceCommandRequest;
import com.huaweicloud.sdk.iotda.v5.region.IoTDARegion;


public class CommandSolution {
    // REGION_ID：如果是上海一，请填写"cn-east-3"；如果是北京四，请填写"cn-north-4";如果是华南广州，请填写"cn-south-4"
    private static final String REGION_ID = "cn-north-4";
    // ENDPOINT：请在控制台的"总览"界面的"平台接入地址"中查看“应用侧”的https接入地址。
    private static final String ENDPOINT = "9d1b8ed274.st1.iotda-app.cn-north-4.myhuaweicloud.com";
    // 标准版/企业版：需自行创建Region对象
    public static final Region REGION_CN_NORTH_4 = new Region(REGION_ID, ENDPOINT);
    public static void main(String[] args) {
        String ak = "BHV9N5RYXZP5ZWO0JAVK";
        String sk = "gcoUuOD6HllHi76j1IvPMEc0mb3WHeI26Yg4SsFZ";
        String projectId = "4e878bb1d8054bb5b1dafe8096e90e42";
        // 创建认证
        ICredential auth = new BasicCredentials().withDerivedPredicate(AbstractCredentials.DEFAULT_DERIVED_PREDICATE)
                .withAk(ak)
                .withSk(sk)
                .withProjectId(projectId);
        // 创建IoTDAClient实例并初始化
        IoTDAClient client = IoTDAClient.newBuilder().withCredential(auth)
                // 基础版：请选择IoTDARegion中的Region对象
                //.withRegion(IoTDARegion.CN_NORTH_4)
                // 标准版/企业版：需自行创建Region对象
                .withRegion(REGION_CN_NORTH_4).build();
        // 实例化请求对象
        CreateCommandRequest request = new CreateCommandRequest();
        request.withDeviceId("658194fe585c81787ad7cfee_20231219");

        DeviceCommandRequest body = new DeviceCommandRequest();
        //这一步文档没有提到但是必须使用
        body.withCommandName("command_total");
        body.withParas("{\"Light\":\"ON\"}");
        request.withBody(body);
        System.out.println(request.getBody());
        System.out.println(client);
        try {
            CreateCommandResponse response = client.createCommand(request);
            System.out.println(response.toString());
        } catch (ConnectionException e) {
            e.printStackTrace();
        } catch (RequestTimeoutException e) {
            e.printStackTrace();
        } catch (ServiceResponseException e) {
            e.printStackTrace();
            System.out.println(e.getHttpStatusCode());
            System.out.println(e.getRequestId());
            System.out.println(e.getErrorCode());
            System.out.println(e.getErrorMsg());
        }
    }
}

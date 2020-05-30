# 有道云笔记自动登陆签到看广告获取空间，配合腾讯云函数python3.6
import requests
# server酱开关，填0不开启(默认)，填1只开启cookie失效通知，填2同时开启cookie失效通知和签到成功通知
sever = ''
# 填写server酱sckey,不开启server酱则不用填
sckey = ''
# 填入有道云笔记客户端抓包的cookie
cookie = ''

def start():
  ad=0
  payload = 'yaohuo:id34976'
  headers = {'Cookie': cookie}
  re = requests.request("POST", "https://note.youdao.com/yws/api/daupromotion?method=sync", headers=headers, data = payload)
  if 'error' not in re.text:
    res = requests.request("POST", "https://note.youdao.com/yws/mapi/user?method=checkin", headers=headers, data = payload)
    for i in range(3):
      resp = requests.request("POST", "https://note.youdao.com/yws/mapi/user?method=adRandomPrompt", headers=headers, data = payload)
      ad += resp.json()['space'] // 1048576
    print(re.text, res.text, resp.text)
    if sever == '2':
      if 'reward' in re.text:
        sync = re.json()['rewardSpace'] // 1048576
        checkin = res.json()['space'] // 1048576
        requests.get('https://sc.ftqq.com/' + sckey + '.send?text=有道云笔记签到成功共获得空间' + str(sync + checkin + ad) + 'M')
  else:
    if sever != '0':
      requests.get('https://sc.ftqq.com/' + sckey + '.send?text=有道云笔记签到cookie失效请更新')


def main_handler(event, context):
  return start()

if __name__ == '__main__':
  start()

